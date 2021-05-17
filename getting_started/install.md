# Установка

## Введение

Сейчас мы создадим структуру репозиториев, которая поможет легко и быстро разворачивать Egal Server.

## Первые шаги

1. Создаем новый репозиторий (в дальнейшем будет называться - Develop),
   в котором мы будем хранить всю кодовую базу проекта.
2. Клонируем его в нужную нам директорию и открываем в IDE
3. Создаем файл `.env`, содержанием которого на данном шаге будет:

```dotenv
PROJECT_NAME=project
```

4. Создаем файл `docker-compose.yml`, содержанием которого на данном шаге будет:

```yaml
version: "3.6"

services:

  # Нужен для общения с сервисами через AMQP, WS протоколы. 
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
      
  # Нужен для общения с сервисами через HTTP протокол.
  web-service: 
    container_name: ${PROJECT_NAME}-web-service
    image: egalbox/web-service:2.0.0beta19
    ports:
      - ${WEB_SERVICE_PORT:-80}:8080
    environment:
      PROJECT_NAME: ${PROJECT_NAME}
      APP_SERVICE_NAME: web
      RABBITMQ_HOST: ${PROJECT_NAME}-rabbitmq
      RABBITMQ_USER: ${RABBITMQ_USERNAME:-admin}
      RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
      WAIT_HOSTS: ${PROJECT_NAME}-rabbitmq:5672
    depends_on:
      - rabbitmq
```

5. Запустим сервисы:

```shell
docker-compose up -d
```

6. Проверим работоспособность `web-service`:

```shell
curl localhost:80
```

Мы должны получить примерно такой ответ:

```html
Hello, it's project/web-service!
```

> Если ответ не получен — дождитесь полного старта сервисов.
> Если в течение 1 минут сервисы не запустились — проверьте docker-compose logs и попробуйте решить проблему.
> Если проблема не решена — обратитесь к команде Egal.

## Добавление стандартных сервисов Egal

На примере `auth-service`, мы попробуем дополнить наше приложение новым сервисом.

1. Дополним `docker-compose.yml`, секцию `services` следующими сервисами:

```yaml
# Нужен для работы сервисов, которые взаимодействуют с базой данных
# На данном шаге нужен только для auth-service
  database:
    container_name: ${PROJECT_NAME}-database
    image: egalbox/postgres:2.0.0
    restart: always
    ports:
      - ${RABBITMQ_PORT:-5432}:5432
    environment:
      POSTGRES_MULTIPLE_DATABASES: auth
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD:-password}

  auth-service:
    container_name: ${PROJECT_NAME}-auth-service
    image: egalbox/auth-service:2.0.0beta20
    environment:
      APP_SERVICE_NAME: auth
      APP_SERVICE_KEY: uZsLnAJz35FWUTVx@eg#Xirv6I*jcw2Y
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

2. Запустим сервисы:

```shell
docker-compose up -d
```

3. Проверим работоспособность `auth-service`

```shell
curl localhost:80/auth/User/getItems
```

Мы должны получить ответ, содержащий `action_result` или `action_error`, это будет означать, что наш сервис ответил.

## Добавление собственных сервисов

На примере `monolit-service`, мы попробуем дополнить наше приложение новым сервисом.

1. Инициализируем composer проект:

```shell
composer create-project egal/egal monolit-service
```

> Если вы получите ошибку `Could not find package egal/egal with stability stable.` - выполните следующую команду вместо предыдущей:
> ```shell
> composer create-project egal/egal monolit-service [VERSION] --stability dev
> ```
> Где `VERSION` - необходимая вам не стабильная версия.

2. Дополним `docker-compose.yml`, секцию `services` следующими сервисами:

```yaml
# Нужен для работы сервисов, которые взаимодействуют с базой данных
# На данном шаге нужен только для auth-service
# Если у вас уже есть database - можно просто добавить название новой базы данных в POSTGRES_MULTIPLE_DATABASES переменной через запятую
  database:
    container_name: ${PROJECT_NAME}-database
    image: egalbox/postgres:2.0.0
    restart: always
    ports:
      - ${RABBITMQ_PORT:-5432}:5432
    environment:
      POSTGRES_MULTIPLE_DATABASES: monolit
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD:-password}

  monolit-service:
    container_name: ${PROJECT_NAME}-monolit-service
    build:
      context: monolit-service
    environment:
      APP_SERVICE_NAME: monolit
      APP_SERVICE_KEY: uiA3ZsU#x@ecwgJrv6ITjLV*nzX5FW2Y
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

3. Запустим сервисы:

```shell
docker-compose up -d
```

4. Проверим работоспособность `monolit-service`

```shell
curl localhost:80/monolit/User/test
```

5. Превращаем `monolit-service` директорию в git submodule Develop репозитория:

    1. Перемещаем кодовую базу во временную директорию:
       
        ```shell
        mv monolit-service/ temp/
        ```
       
    2. Делаем commit текущего репозитория без `/temp` директории.
       
       > В изменениях должно отображаться удаление директории `/monolit-service`

    3. Инициализируем git submodules, если ранее не были инициализированы:
       
        ```shell
        git submodule init
        ```
       
    4. Инициализируем monolit-service submodule:

        ```shell
        git submodule add [URL] monolit-service
        ```
        > Где URL - HTTP или SSH ссылка на Git репозиторий Monolit Service. Репозиторий должен быть не пустым.

        > Если вы используете IDE JetBrains PhpStorm - на данном этапе его лучше перезапустить,
        > чтобы прошла заново индексация проекта.

    5. Актуализируем кодовую базу Monolit Service репозитория:
       
        ```shell
        rsync -r ./temp/ ./monolit-service/
        ```
       
    6. Удаляем временную `/temp` директорию:
       
        ```shell
        rm -r temp
        ```
       
    7. Далее делаем commit всех новых файлов как в Develop, так и в Monolit Service.
    
## Актуализация кодовой базы сервиса

На примере Monolit Service добавим возможность перманентной актуализации кодовой базы.

1. Добавляем `volume` в сервис docker-compose.yml:

    ```yaml
      monolit-service:
        # ...
        volumes:
          - PATH_TO_SERVICE_DIR:/app
        # ...
    ```
    
    > Где PATH_TO_SERVICE_DIR - директория кодовой базы сервиса.

2. Перезапускаем контейнер

    ```shell
    docker-compose up -d --force-recreate --no-deps monolit-service
    ```

> Если сервис запущен иным способом в отличие от стандартной команды CMD - перезапустите его соответствующим способом
> текущей реализации запуска.

> При каждом изменении кодовой базы для её применения необходимо перезапустить сервис.

