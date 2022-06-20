# Установка

## Введение

Создадим структуру репозиториев, которая поможет легко и быстро
разворачивать Egal Server.


## Первые шаги

1. Создаем новый репозиторий (в дальнейшем - Develop) для хранения
   кодовой базы проекта. При создании проекта не выбираем пункт `Initialize repository with a README`
2. Клонируем его в нужную нам директорию и открываем в IDE
3. В открытую директорию загружаем скрипт установки:
   
```shell
wget https://github.com/egal/installer/releases/download/v2.0.0-beta.15/egal-installer-v2.0.0-beta.15
```

>Скрипт установки инициализирует кодовую базу проекта на основе фреймворка Egal.

4. Выдаем для скрипта установки право на исполнение
   
```shell
chmod +x ./egal-installer-v2.0.0-beta.15
```

5. Запускаем скрипт:

```shell
./egal-installer-v2.0.0-beta.15
```

> При наличии в текущей директории нескрытых файлов, за исключением скрипта установки, скрипт выдаст ошибку:
> `Directory is not empty!`
> Необходимо удалить все файлы из текущей директории, оставив только скрытые файлы и файл скрипта установки.
   
Вводим имя проекта:

```html
? Enter project name: 
```
   
Выбираем тип клиента:

```html
? What type of client you need? (Use arrow keys)
  » Vue.js
    Nuxt.js
```

Выбираем тип сборки сервиса авторизации (`auth-service`):

```html
? What build type of auth-service you need? (Use arrow keys)
 » Build from image
   Build from context
```

> `Build from image` - способ сборки сервиса на основе образа удаленного репозитория.
> Такой способ подходит, когда нет необходимости конфигурировать сервис авторизации.
> 
> `Build from context` - способ сборки сервиса на основе контекста. В этом случае инициализируется копия
> удаленного репозитория сервиса авторизации.

Указываем, нужно ли проинициализировать дополнительные сервисы, помимо `auth-service`, ```monolit-service```

```html
? Create new service? (Y/n)
```

При необходимости создать дополнительный сервис, указываем имя сервиса.
Для правильной конфигурации необходимо вводить имя сервиса, оканчивающееся на "-service":

```html
? Enter service name:
```

> В дальнейшем скрипт продолжит спрашивать о необходимости создать
> дополнительно сервис, пока разработчик не ответит `no`

В случае успешного завершения выполнения скрипт должен вывести ответ:

```html
Completed!
```

Из текущей директории можно удалить скрипт установки.

> В случае возникновения непредвиденных ситуаций, прерывающих выполнение скрипта установки,
> необходимо очистить текущую директорию и повторно запустить скрипт.

6. Соберем и запустим сервисы:

```shell
docker-compose up -d --build
```
   
7. Проверим работоспособность `web-service`:

```shell
curl localhost:80
```

Должны получить примерно такой ответ:

```html
Hello, it's project/web-service!
```

> Если ответ не получен — нужно дождаться полного старта сервисов.
>
> Если в течение 1 минуты сервисы не запустились — проверьте
> docker-compose logs. Если проблема не решена — нужно обратиться к
> команде Egal.


## Добавление собственных сервисов

На примере `monolit-service` попробуем дополнить приложение новым
сервисом

1. Устанавливаем сервис через composer:

```shell
composer create-project egal/egal monolit-service
```

> Если выводится ошибка `Could not find package egal/egal with
> stability stable.` - выполняется следующая команда:
>
> ```shell
> composer create-project egal/egal monolit-service [VERSION] --stability dev
> ```
>
> Где `VERSION` - необходимая вам неcтабильная версия.

2. Дополним `docker-compose.yml`, секцию `services` сервисами:

```yaml
# Нужен для работы сервисов, которые взаимодействуют с базой данных
# На данном шаге нужен только для auth-service
# Если уже есть в postgres - можно добавить название новой базы данных в POSTGRES_MULTIPLE_DATABASES переменной через запятую
  postgres:
    image: egalbox/postgres:2.1.0
    restart: unless-stopped
    environment:
      POSTGRES_MULTIPLE_DATABASES: auth, monolit
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD:-password}
  monolit-service:
    build:
      context: server/monolit-service
    restart: unless-stopped
    depends_on:
       - rabbitmq
       - database
    environment:
      APP_NAME: ${PROJECT_NAME}
      APP_SERVICE_NAME: monolit
      APP_SERVICE_KEY: uiA3ZsU#x@ecwgJrv6ITjLV*nzX5FW2Y
      DB_HOST: postgres
      DB_USERNAME: ${DB_USERNAME}
      DB_PASSWORD: ${DATABASE_PASSWORD:-password}
      RABBITMQ_HOST: rabbitmq
      RABBITMQ_USER: ${RABBITMQ_USER:-user}
      RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
      WAIT_HOSTS: rabbitmq:5672,postgres:5432
```

3. Запустим сервисы:

```shell
docker-compose up -d
```

4. Проверим работоспособность `monolit-service`

```shell
curl localhost:80/monolit/MODEL_NAME/ACTION_NAME
```

> MODEL_NAME - название модели внутри Monolit Service.
>
> ACTION_NAME - название action у MODEL_NAME внутри Monolit Service.

5. Преобразуем `monolit-service` директорию в git submodule Develop
репозитория:

   1. Перемещаем кодовую базу во временную директорию:

   ```shell
   mv monolit-service/ temp/
   ```

   2. Делаем commit текущего репозитория без `/temp` директории.

   > В изменениях должно отображаться удаление директории
   > `/monolit-service`

   3. Инициализируем git submodules, если не инициализированы:

   ```shell
   git submodule init
   ```

   4. Инициализируем monolit-service submodule:

   ```shell
   git submodule add [URL] monolit-service
   ```

   > Где URL - HTTP или SSH ссылка на Git репозиторий Monolit
   > Service. Репозиторий должен быть не пустым.
   >
   > Если вы используете IDE JetBrains PhpStorm - на данном этапе его
   > лучше перезапустить, чтобы заново прошла индексация проекта.

   5. Актуализируем кодовую базу Monolit Service репозитория:

   ```shell
   rsync -r ./temp/ ./monolit-service/
   ```

   6. Удаляем временную `/temp` директорию:

   ```shell
   rm -r temp
   ```

   7. Делаем commit всех новых файлов как в Develop, так и в Monolit
      Service.


## Актуализация кодовой базы сервиса

На примере Monolit Service добавим возможность перманентной актуализации
кодовой базы.

1. Добавим `volume` в сервис docker-compose.yml:

```yaml
monolit-service:
// ...
 volumes:
   - PATH_TO_SERVICE_DIR:/app
 # ...
```

> Где PATH_TO_SERVICE_DIR - директория кодовой базы сервиса.

2. Перезапустим контейнер

```shell
docker-compose up -d --force-recreate --no-deps monolit-service
```

> Если сервис запущен не стандартной командой CMD - перезапустите его
> соответствующим способом текущей реализации запуска.
>
> Для применения изменения кодовой базы требуется перезапустить всех
> слушателей сервиса.

