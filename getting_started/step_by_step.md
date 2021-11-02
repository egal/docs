# Пример по шагам

В этом разделе представлен пример создания части проекта "Онлайн
платформы для обучения". Для простоты рассмотрим создание сущностей
`WorkingTime` и `Speaker`.

> Проект — платформа для организации и проведения видео-уроков.

Проект будет состоять из:
- Monolit - основной сервис в котором будет наше приложение.
- Сервис авторизации.
- Веб-сервис.

Статья состоит из следующих частей:
1. Разворот проекта.
2. Создание сущностей.
3. Авторизация.


## 1. Разворот проекта

Инициализируем `monolit`.

<!-- TODO: Описать как инициализировать -->

Получаем структуру директории:

```text
monolit-service/
docker-compose.yml
assistent             скрипт помощник для работы с проектом
.gitignore
.gitmodules           файл с описанием существующих подмодулей
.git                  системная директория вашего репозитория
.env                  файл конфигурации c частью переменных окружения
```

Установим зависимости. В директории `monolit-service` выполняем:

```shell
docker run --rm --interactive --tty --volume $PWD:/app --user $(id -u):$(id -g) composer install --ignore-platform-reqs
```

Настраиваем docker-compose.yml

Удаляем ненужные закомментированные сервисы в начале файла (такие, как
pgadmin, k6, php-documentor). Содержимое файла должно быть таким:

```yaml
version: "3.6"

services:
  rabbitmq:
    container_name: ${PROJECT_NAME}-rabbitmq
    image: ${RABBITMQ_TAG:-bitnami/rabbitmq:latest}
    environment:
      RABBITMQ_USERNAME: ${RABBITMQ_USERNAME:-admin}
      RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
      RABBITMQ_PLUGINS: rabbitmq_management,rabbitmq_consistent_hash_exchange
    ports:
      - ${RABBITMQ_PORT:-5672}:5672
      - ${RABBITMQ_MANAGER_PORT:-15672}:15672
```

В конец файла добавляем все необходимое:
1. База данных.

```yaml
    database:
      container_name: ${PROJECT_NAME}-database
      image: egalbox/postgres:2.0.0 # указываем адрес, откуда подтягивается образ контейнера
      restart: always # всегда перезапускаем контейнер
      ports:
        - ${RABBITMQ_PORT:-5432}:5432
      environment:
        POSTGRES_MULTIPLE_DATABASES: auth,monolit # через запятую указываем название баз данных, используемых в проекте. Эта переменная нестандартная и работает только с образом egalbox/postgres:2.0.0.
        POSTGRES_USER: postgres
        POSTGRES_PASSWORD: ${DATABASE_PASSWORD:-password}
```

2. Сервисы, поставляемые с `egal-box`

```yaml
 web-service:
   container_name: ${PROJECT_NAME}-web-service
   image: egalbox/web-service:2.0.0beta19
   ports:
     - ${WEB_SERVICE_PORT:-81}:8080
   environment:
     PROJECT_NAME: ${PROJECT_NAME}
     APP_SERVICE_NAME: web
     RABBITMQ_HOST: ${PROJECT_NAME}-rabbitmq
     RABBITMQ_USER: ${RABBITMQ_USERNAME:-admin}
     RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
     WAIT_HOSTS: ${PROJECT_NAME}-rabbitmq:5672
   depends_on:
     - rabbitmq

 auth-service:
  container_name: ${PROJECT_NAME}-auth-service
  image: egalbox/auth-service:2.0.0beta20
  environment:
    APP_SERVICE_NAME: auth
    APP_SERVICE_KEY: uZn35FJAx@sg*eczrv6ITjLVWU#Xiw2Y
    APP_SERVICES: monolit:B#J5mUWKh8FqzQ6Tj0XtYruIcSwpb@ed
    DB_HOST: ${PROJECT_NAME}-database
    DB_PASSWORD: ${DATABASE_PASSWORD:-password}
    RABBITMQ_HOST: ${PROJECT_NAME}-rabbitmq
    RABBITMQ_USER: ${RABBITMQ_USERNAME:-admin}
    RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
    WAIT_HOSTS: ${PROJECT_NAME}-rabbitmq:5672, ${PROJECT_NAME}-database:5432
  depends_on:
    - rabbitmq
    - database
```

3. Сервис `monolit`

```yaml
 monolit-service:
   container_name: ${PROJECT_NAME}-monolit-service
   build: monolit-service
   volumes:
     - ./monolit-service/:/app/
   environment:
     APP_SERVICE_NAME: monolit
     APP_SERVICE_KEY: B#J5mUWKh8FqzQ6Tj0XtYruIcSwpb@ed
     DB_HOST: ${PROJECT_NAME}-database
     DB_PASSWORD: ${DATABASE_PASSWORD:-password}
     RABBITMQ_HOST: ${PROJECT_NAME}-rabbitmq
     RABBITMQ_USER: ${RABBITMQ_USERNAME:-admin}
     RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
     WAIT_HOSTS: ${PROJECT_NAME}-rabbitmq:5672, ${PROJECT_NAME}-database:5432
   depends_on:
     - rabbitmq
     - database
```


### Настройка .env

Переменная `PROJECT_NAME` определяет название проекта.

```dotenv
PROJECT_NAME=testProject
```


### Миграции

Миграции выполнятся при старте сервиса Docker Compose, если в блоке
`CMD` указана команда запуска этих миграций. В стандартном Dockerfile
она указана.

```
CMD /wait && ./artisan migrate --force && ./artisan egal:run
```

> Либо если по какой-то причине в Dockerfile это не прописано, то
> вручную запустить миграции можно вот так:
>
> Заходим в контейнер и запускаем миграции:
>
> ```shell
> docker-compose exec monolit-service php artisan migrate
> ```


### Запуск проекта

Запуск производится командой

```shell
docker-compose up -d
```

Для проверки работоспособности используем команду вывода списка
работающих контейнеров.

```shell
docker-compose ps
```

Примерный результат

```text
            Name                           Command               State                                             Ports                                           
-------------------------------------------------------------------------------------------------------------------------------------------------------------------
testProject-auth-service        docker-php-entrypoint /bin ...   Up                                                                                                
testProject-database            docker-entrypoint.sh postgres    Up      5432/tcp                                                                                  
testProject-monolit-service     docker-php-entrypoint /bin ...   Up                                                                                                
testProject-rabbitmq            /opt/bitnami/scripts/rabbi ...   Up      15671/tcp, 0.0.0.0:15672->15672/tcp, 25672/tcp, 4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp
testProject-web-service         /bin/sh -c /wait && /usr/b ...   Up      0.0.0.0:81->8080/tcp                                                                      

```

Сделаем запрос для проверки.

```shell
curl http://localhost:81
```

Ответ

```text
Hello, it's testProject/web-service!
```

## 2. Создание сущностей

Начнем с генерации модели `Speaker`. Создадим модель в контейнере

```shell
docker-compose exec monolit-service bash
php artisan egal:make:model Speaker
```

После выполнения этой команды у нас сгенерировалась модель со следующим
содержимым:

```php
<?php

namespace App\Models;

use Egal\Model\Model as EgalModel;

/**
 * @property $id
 * @property $name {@validation-rules required|string}
 * @property $created_at
 * @property $updated_at
 *
 * @action getMetadata {@statuses-access guest|logged}
 * @action getItem {@statuses-access guest|logged}
 * @action getItems {@statuses-access logged} {@roles-access user}
 * @action create {@statuses-access logged} {@permissions-access super_permission}
 * @action update {@statuses-access logged} {@permissions-access super_permission}
 * @action delete {@statuses-access logged} {@permissions-access super_permission}
 */
class Speaker extends EgalModel
{

}
```

Аналогично создадим сущность `WorkingTime`.

```shell
php artisan egal:make:model WorkingTime
```

Внесем правки:

- Временно для удобства сделаем все роуты публичными
- Укажем типы полей и валидацию
- Укажем связанные сущности. Для `WorkingTime` это `speaker`, для
  `Speaker` это `workingTimes`.

В итоге получаем:

`app/Models/Speaker.php`

```php
<?php

namespace App\Models;

use Carbon\Carbon;
use Egal\Model\Model as EgalModel;
use Illuminate\Database\Eloquent\Collection;
use Illuminate\Database\Eloquent\Relations\HasMany;

/**
 * @property integer $id           {@property-type field}  {@primary-key}
 * @property uuid $uid             {@property-type field}  {@validation-rules required|uuid|unique:speakers,uid|unique:schools,uid}
 * @property string $name          {@property-type field}  {@validation-rules required|string}
 * @property string $surname       {@property-type field}  {@validation-rules required|string}
 * @property string $avatar        {@property-type field}  {@validation-rules string}
 * @property string $video         {@property-type field}  {@validation-rules string}
 * @property Carbon $created_at    {@property-type field}
 * @property Carbon $updated_at    {@property-type field}
 *
 * @property Collection|WorkingTime[] $workingTimes     {@property-type relation}
 *
 * @action getMetadata  {@statuses-access guest|logged}
 * @action getItem      {@statuses-access guest|logged}
 * @action getItems     {@statuses-access guest|logged}
 * @action create       {@statuses-access guest|logged}
 * @action update       {@statuses-access guest|logged}
 * @action delete       {@statuses-access guest|logged}
 */
class Speaker extends EgalModel
{

    protected $fillable = [
        'uid',
        'name',
        'surname',
        'avatar',
        'video',
    ];

    protected $hidden = [
        'created_at',
        'updated_at',
    ];
    
    public function workingTimes(): HasMany
    {
        return $this->hasMany(WorkingTime::class);
    }

}

```

`app/Models/WorkingTime.php`

```php
<?php

namespace App\Models;

use Carbon\Carbon;
use Egal\Model\Model as EgalModel;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

/**
 * @property integer $id           {@property-type field}  {@primary-key}
 * @property integer $speaker_id   {@property-type field}  {@validation-rules required|exists:speakers,id}
 * @property Carbon $starts_at     {@property-type field}  {@validation-rules required|date|before:ends_at}
 * @property Carbon $ends_at       {@property-type field}  {@validation-rules required|date|after:starts_at}
 * @property Carbon $created_at    {@property-type field}
 * @property Carbon $updated_at    {@property-type field}
 *
 * @property Speaker $speaker {@property-type relation}
 *
 * @action getMetadata          {@statuses-access guest|logged}
 * @action create               {@statuses-access guest|logged}
 * @action update               {@statuses-access guest|logged}
 * @action delete               {@statuses-access guest|logged}
 * @action actionCreateMany     {@statuses-access guest|logged}
 * @action actionDeleteManyRaw  {@statuses-access guest|logged}
 * @action getItem              {@statuses-access guest|logged}
 * @action getItems             {@statuses-access guest|logged}
 */
class WorkingTime extends EgalModel
{

    protected $fillable = [
        'speaker_id',
        'starts_at',
        'ends_at',
    ];

    protected $hidden = [
        'created_at',
        'updated_at',
    ];

    public function speaker(): BelongsTo
    {
        return $this->belongsTo(Speaker::class);
    }
    
}

```

Далее сгенерируем миграции для этих сущностей

```shell
php artisan egal:make:migration-create Speaker
php artisan egal:make:migration-create WorkingTime
```

Пропишем в них связи на таблицы

`database/migrations/2021_04_27_064152_create_speakers_table.php`

```php
class CreateSpeakersTable extends Migration
{
    public function up()
        Schema::create('speakers', function (Blueprint $table) {
            $table->id();
            $table->uuid('uid')
                ->unique();
            $table->string('name');
            $table->string('surname');
            $table->string('avatar')
                ->nullable();
            $table->string('video')
                ->nullable();
            $table->timestamps();
        });
    }
/** ... */
}
```

`database/migrations/2021_04_27_064229_create_working_times_table.php`

```php
class CreateWorkingTimesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('working_times', function (Blueprint $table) {
            $table->id();
            $table->foreignId('speaker_id')
                ->constrained()
                ->onDelete('cascade');
            $table->timestamp('starts_at');
            $table->timestamp('ends_at');
            $table->timestamps();
        });
    }
/** ... */
}
```

Для выполнения миграций и применения нового кода перезапустим сервис
`monolit`

```shell
docker-coompose restart monolit-service
```

Для проверки работоспособности сделаем пару запросов на создание
`WorkingTime` и `Speaker`.


```shell
curl -iLXPOST -H "Content-Type: application/json" -d '{"attributes": {"name": "Ivan", "surname": "Ivanov"}}' http://localhost:81/monolit/Speaker/create
curl -iLXPOST -H "Content-Type: application/json" -d '{"attributes": {"speaker_id": "1", "starts_at": "2021-04-27 03:23:57", "ends_at": "2021-04-28 03:23:57"}}' http://localhost:81/monolit/WorkingTime/create
```

Запросим созданные данные

```shell
curl http://localhost:81/monolit/Speaker/getItems
```

Результат

```json
{
  "action": {
    "response": {},
    "connection": {},
    "type": "action",
    "service_name": "monolit",
    "model_name": "Speaker",
    "action_name": "getItems",
    "parameters": [],
    "token": null,
    "uuid": "710b36c0-77fb-4906-a4c2-234bf53fb09a"
  },
  "start_processing": {
    "type": "start_processing",
    "started_at": "2021-04-28T03:38:18.464676Z",
    "uuid": "5ae27e60-d05b-4b11-97eb-80ea0a81ecac",
    "action_message": {
      "type": "action",
      "service_name": "monolit",
      "model_name": "Speaker",
      "action_name": "getItems",
      "parameters": [],
      "token": null,
      "uuid": "710b36c0-77fb-4906-a4c2-234bf53fb09a"
    }
  },
  "action_result": {
    "type": "action_result",
    "data": {
      "current_page": 1,
      "total_count": 1,
      "per_page": 10,
      "items": [
        {
          "id": 1,
          "name": "Ivan",
          "surname": "Ivanov",
          "avatar": null,
          "video": null
        }
      ]
    },
    "uuid": "42e389bb-30d6-4483-bd84-7be78a5654dc",
    "action_message": {
      "type": "action",
      "service_name": "monolit",
      "model_name": "Speaker",
      "action_name": "getItems",
      "parameters": [],
      "token": null,
      "uuid": "710b36c0-77fb-4906-a4c2-234bf53fb09a"
    }
  },
  "action_error": null
}
```

Создание `WorkingTime`

```shell
curl -iLXPOST -H "Content-Type: application/json" -d '{"attributes": {"speaker_id": "1", "starts_at": "2021-04-27 03:23:57", "ends_at": "2021-04-28 03:23:57"}}' http://localhost:81/monolit/WorkingTime/create
```

Получаем результат

```shell
curl http://localhost:81/monolit/WorkingTime/getItems
```


## 3. Авторизация

Закроем доступ для незарегистрированных пользователей: уберем у каждой
модели разрешения для guest - изменим `{@statuses-access guest|logged}`
на `{@statuses-access logged}`

Зарегистрируем нового пользователя.

```shell
curl -iLXPOST -H "Content-Type: application/json" -d '{"email": "ivan@mail.ru", "password": "qazwsx"}' http://localhost:81/auth/User/register
```

Получаем user master token

```shell
curl -iLXPOST -H "Content-Type: application/json" -d '{"email": "ivan@mail.ru", "password": "qazwsx"}' http://localhost:81/auth/User/login
```

Ответ:

```json
{
  "action": {
    "type": "action",
    "service_name": "auth",
    "model_name": "User",
    "action_name": "login",
    "parameters": {
      "email": "ivan@mail.ru",
      "password": "qazwsx"
    },
    "token": null,
    "uuid": "e47f2afa-1902-43d2-ade5-c1f17ec51312"
  },
  "start_processing": {
    "type": "start_processing",
    "started_at": "2021-11-02T08:39:28.673032Z",
    "uuid": "c055b157-f582-4f7e-8a18-f643681f031c",
    "action_message": {
      "type": "action",
      "service_name": "auth",
      "model_name": "User",
      "action_name": "login",
      "parameters": {
        "email": "ivan@mail.ru",
        "password": "qazwsx"
      },
      "token": null,
      "uuid": "e47f2afa-1902-43d2-ade5-c1f17ec51312"
    }
  },
  "action_result": {
    "type": "action_result",
    "data": {
      "user_master_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImFsaXZlX3VudGlsIjoiMjAyMS0xMS0wM1QwODozOToyOC43Mjk1NTBaIn0.7cWYVaMRl8Mzq-ahMWobl-WKzTTJTo2AIHXV9kA6PGE",
      "user_master_refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW1ydCIsImF1dGhfaWRlbnRpZmljYXRpb24iOiI3MzBhNTE5NS1mNmE5LTQ0ZGQtYWNiMS1mYTZkYTQ4YWZlMzgiLCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDlUMDg6Mzk6MjguNzI5NjMyWiJ9.F7GEfMMXk1JcYgQUlaZ81mrAW_ua-4uodQUAox--KNk"
    },
    "uuid": "8430ed91-7a7e-41ab-8d97-bcf194bfc19b",
    "action_message": {
      "type": "action",
      "service_name": "auth",
      "model_name": "User",
      "action_name": "login",
      "parameters": {
        "email": "ivan@mail.ru",
        "password": "qazwsx"
      },
      "token": null,
      "uuid": "e47f2afa-1902-43d2-ade5-c1f17ec51312"
    }
  },
  "action_error": null
}
```

Нам нужен токен в поле `data`:

```text
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjU1MDcwNjQzLTMzOTAtNDM4My1hYzA0LWM3ODM4NzdmZGYwMiIsImFsaXZlX3VudGlsIjoiMjAyMS0wNC0yOVQwODowMDoyMi4zMjEyNzJaIn0.8MNzIB137LC1QYIOt7Io3zTfSO9xUbklaTn5xB_7yP4
```

Теперь нужно получить user service token, чтобы мы могли делать запросы на
защищенные пути нашего сервиса. В данный запрос необходимо передать полученный user master token:

```shell
curl -iLXPOST -H "Content-Type: application/json" -d '{"token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImFsaXZlX3VudGlsIjoiMjAyMS0xMS0wM1QwODozOToyOC43Mjk1NTBaIn0.7cWYVaMRl8Mzq-ahMWobl-WKzTTJTo2AIHXV9kA6PGE", "service_name": "monolit"}' http://localhost:81/auth/User/loginToService
```

Ответ

```json
{
  "action": {
    "type": "action",
    "service_name": "auth",
    "model_name": "User",
    "action_name": "loginToService",
    "parameters": {
      "service_name": "core",
      "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImFsaXZlX3VudGlsIjoiMjAyMS0xMS0wM1QwODozOToyOC43Mjk1NTBaIn0.7cWYVaMRl8Mzq-ahMWobl-WKzTTJTo2AIHXV9kA6PGE"
    },
    "token": null,
    "uuid": "2e4ad88f-ad11-4b84-b8fa-e2a6c4d18cd4"
  },
  "start_processing": {
    "type": "start_processing",
    "started_at": "2021-11-02T08:41:21.140022Z",
    "uuid": "c5321643-3e9f-4e03-b617-ed5e953afd31",
    "action_message": {
      "type": "action",
      "service_name": "auth",
      "model_name": "User",
      "action_name": "loginToService",
      "parameters": {
        "service_name": "core",
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImFsaXZlX3VudGlsIjoiMjAyMS0xMS0wM1QwODozOToyOC43Mjk1NTBaIn0.7cWYVaMRl8Mzq-ahMWobl-WKzTTJTo2AIHXV9kA6PGE"
      },
      "token": null,
      "uuid": "2e4ad88f-ad11-4b84-b8fa-e2a6c4d18cd4"
    }
  },
  "action_result": {
    "type": "action_result",
    "data": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidXN0IiwiYXV0aF9pbmZvcm1hdGlvbiI6eyJpZCI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImVtYWlsIjoiaXZhbkBtYWlsLnJ1IiwiY3JlYXRlZF9hdCI6IjIwMjEtMTEtMDJUMDg6Mzk6MDguMDAwMDAwWiIsInVwZGF0ZWRfYXQiOiIyMDIxLTExLTAyVDA4OjM5OjA4LjAwMDAwMFoiLCJhdXRoX2lkZW50aWZpY2F0aW9uIjoiNzMwYTUxOTUtZjZhOS00NGRkLWFjYjEtZmE2ZGE0OGFmZTM4Iiwicm9sZXMiOltdLCJwZXJtaXNzaW9ucyI6W119LCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDJUMDg6NTE6MjEuMTQxNzg5WiJ9.sG8V6360lX8hWkcPHkr2VpGbxU5vXeQ-4CcjuKJJb1s",
    "uuid": "9595d2b7-f844-4391-bbe6-e69e83b5afd6",
    "action_message": {
      "type": "action",
      "service_name": "auth",
      "model_name": "User",
      "action_name": "loginToService",
      "parameters": {
        "service_name": "core",
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImFsaXZlX3VudGlsIjoiMjAyMS0xMS0wM1QwODozOToyOC43Mjk1NTBaIn0.7cWYVaMRl8Mzq-ahMWobl-WKzTTJTo2AIHXV9kA6PGE"
      },
      "token": null,
      "uuid": "2e4ad88f-ad11-4b84-b8fa-e2a6c4d18cd4"
    }
  },
  "action_error": null
}
```

Попробуем сделать запрос в наш сервис с полученным user service token:

```shell
curl -H "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidXN0IiwiYXV0aF9pbmZvcm1hdGlvbiI6eyJpZCI6IjU1MDcwNjQzLTMzOTAtNDM4My1hYzA0LWM3ODM4NzdmZGYwMiIsImVtYWlsIjoiaXZhbkBtYWlsLnJ1IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjU1MDcwNjQzLTMzOTAtNDM4My1hYzA0LWM3ODM4NzdmZGYwMiIsInJvbGVzIjpbXSwicGVybWlzc2lvbnMiOltdfSwiYWxpdmVfdW50aWwiOiIyMDIxLTA0LTI4VDA4OjExOjM1LjIyMzUzNFoifQ.oonpQvvOAycWg5W2Bkh_fvETQofLs6Wc0J3Y5eT3c04" http://localhost:81/monolit/Speaker/getItems
```

Если все хорошо, то вы увидите тот же самый результат, как и в прошлый
раз. Если возникают ошибки — требуется проверить каждый шаг с самого
начала.

Если user master token устарел, его можно обновить с помощью user master refresh token, полученном в ответе на запрос User/login:

```shell
curl -iLXPOST -H "Content-Type: application/json" -d '{"token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW1ydCIsImF1dGhfaWRlbnRpZmljYXRpb24iOiI3MzBhNTE5NS1mNmE5LTQ0ZGQtYWNiMS1mYTZkYTQ4YWZlMzgiLCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDlUMDg6Mzk6MjguNzI5NjMyWiJ9.F7GEfMMXk1JcYgQUlaZ81mrAW_ua-4uodQUAox--KNk"}' http://localhost:81/auth/User/refreshUserMasterToken
```
В ответ на запрос приходит новая пара user master token и user master refresh token:

```json
{
  "action": {
    "type": "action",
    "service_name": "auth",
    "model_name": "User",
    "action_name": "refreshUserMasterToken",
    "parameters": {
      "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW1ydCIsImF1dGhfaWRlbnRpZmljYXRpb24iOiI3MzBhNTE5NS1mNmE5LTQ0ZGQtYWNiMS1mYTZkYTQ4YWZlMzgiLCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDlUMDg6Mzk6MjguNzI5NjMyWiJ9.F7GEfMMXk1JcYgQUlaZ81mrAW_ua-4uodQUAox--KNk"
    },
    "token": null,
    "uuid": "5640c17a-8ab7-4315-b884-f7ee8c9b5339"
  },
  "start_processing": {
    "type": "start_processing",
    "started_at": "2021-11-02T08:50:57.741453Z",
    "uuid": "0c9eb7ce-6bac-45c0-b8b7-2089c0e33089",
    "action_message": {
      "type": "action",
      "service_name": "auth",
      "model_name": "User",
      "action_name": "refreshUserMasterToken",
      "parameters": {
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW1ydCIsImF1dGhfaWRlbnRpZmljYXRpb24iOiI3MzBhNTE5NS1mNmE5LTQ0ZGQtYWNiMS1mYTZkYTQ4YWZlMzgiLCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDlUMDg6Mzk6MjguNzI5NjMyWiJ9.F7GEfMMXk1JcYgQUlaZ81mrAW_ua-4uodQUAox--KNk"
      },
      "token": null,
      "uuid": "5640c17a-8ab7-4315-b884-f7ee8c9b5339"
    }
  },
  "action_result": {
    "type": "action_result",
    "data": {
      "user_master_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW10IiwiYXV0aF9pZGVudGlmaWNhdGlvbiI6IjczMGE1MTk1LWY2YTktNDRkZC1hY2IxLWZhNmRhNDhhZmUzOCIsImFsaXZlX3VudGlsIjoiMjAyMS0xMS0wM1QwODo1MDo1Ny43NDI5NDVaIn0.RQdL1i2Z5nCoiYfHD73Zm8FoC0d7c6Widklc9H0RSeE",
      "user_master_refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW1ydCIsImF1dGhfaWRlbnRpZmljYXRpb24iOiI3MzBhNTE5NS1mNmE5LTQ0ZGQtYWNiMS1mYTZkYTQ4YWZlMzgiLCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDlUMDg6NTA6NTcuNzQyOTgwWiJ9.Eay6shqrVmh2_-lFq7vYeb_CgTVV5ZP7Xb04okGULak"
    },
    "uuid": "c4cd83c8-96cf-42a7-96bc-a4963ff97502",
    "action_message": {
      "type": "action",
      "service_name": "auth",
      "model_name": "User",
      "action_name": "refreshUserMasterToken",
      "parameters": {
        "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidW1ydCIsImF1dGhfaWRlbnRpZmljYXRpb24iOiI3MzBhNTE5NS1mNmE5LTQ0ZGQtYWNiMS1mYTZkYTQ4YWZlMzgiLCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDlUMDg6Mzk6MjguNzI5NjMyWiJ9.F7GEfMMXk1JcYgQUlaZ81mrAW_ua-4uodQUAox--KNk"
      },
      "token": null,
      "uuid": "5640c17a-8ab7-4315-b884-f7ee8c9b5339"
    }
  },
  "action_error": null
}
```

## Заключение

Основные шаги создания проекта с нуля пройдены. В итоге получена
"заготовка" сервиса с настроенной авторизацией.

