# Установка

## Введение

Создание структуры репозиториев для разворачивания Egal Server.

## Первые шаги

1. Создание нового репозитория (в дальнейшем - Develop)
   для кодовой базы проекта.
2. Его клонирование в нужную директорию и открывается в IDE
3. Создание файла `.env` с содержанием:

    ```dotenv
    PROJECT_NAME=project
    ```

4. Создание файла `docker-compose.yml` с содержанием:

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

5. Запуск сервисов:

    ```shell
    docker-compose up -d
    ```

6. Проверка работоспособности `web-service`:

    ```shell
    curl localhost:80
    ```

    Примерный ответ:
    
    ```html
    Hello, it's project/web-service!
    ```
    
    > Если ответ не получен - нужно ждать полного старта сервисов.
    > Если в течение 1 минуты сервисы не запустились — проверяются docker-compose logs.
    > Если проблема не решена — нужно обратиться к команде Egal.

## Добавление стандартных сервисов Egal

Добавления нового сервиса в приложение на примере `auth-service`

1. Дополнение `docker-compose.yml` в секции `services` сервисами:

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

2. Запуск сервисов:

    ```shell
    docker-compose up -d
    ```

3. Проверка работоспособности `auth-service`

    ```shell
    curl localhost:80/auth/User/getItems
    ```
    
    Ожидается ответ, содержащий `action_result` или `action_error`.

## Добавление собственных сервисов

Добавления нового сервиса в приложение на примере `monolit-service`

1. Инициализация composer проекта:
    
    ```shell
    composer create-project egal/egal monolit-service
    ```
    
    > Если выводится ошибку `Could not find package egal/egal with stability stable.` - выполняется следующая команда:
    > ```shell
    > composer create-project egal/egal monolit-service [VERSION] --stability dev
    > ```
    > Где `VERSION` - необходимая вам неcтабильная версия.
<!-- Точно нестабильная версия? -->

2. Дополнение `docker-compose.yml` в секции `services` сервисами:

    ```yaml
    # Нужен для работы сервисов, которые взаимодействуют с базой данных
    # На данном шаге нужен только для auth-service
    # Если уже есть database - можно добавить название новой базы данных в POSTGRES_MULTIPLE_DATABASES переменной через запятую
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

3. Запуск сервисов:

    ```shell
    docker-compose up -d
    ```

4. Проверка работоспособности `monolit-service`

    ```shell
    curl localhost:80/monolit/MODEL_NAME/ACTION_NAME
    ```
    
    > Где MODEL_NAME - название модели внутри Monolit Service.
     
    > Где ACTION_NAME - название action у MODEL_NAME внутри Monolit Service.

5. Перевод `monolit-service` директорию в git submodule Develop репозитория:

    1. Перемещение кодовой базы во временную директорию:
       
        ```shell
        mv monolit-service/ temp/
        ```
       
    2. Commit текущего репозитория без `/temp` директории.
       
       > В изменениях должно отображаться удаление директории `/monolit-service`

    3. Инициализация git submodules, если не инициализированы:
       
        ```shell
        git submodule init
        ```
       
    4. Инициализация monolit-service submodule:

        ```shell
        git submodule add [URL] monolit-service
        ```
        > Где URL - HTTP или SSH ссылка на Git репозиторий Monolit Service. Репозиторий должен быть не пустым.

        > Если вы используете IDE JetBrains PhpStorm - на данном этапе его лучше перезапустить,
        > чтобы переиндексировать проект.

    5. Актуализация кодовой базы Monolit Service репозитория:
       
        ```shell
        rsync -r ./temp/ ./monolit-service/
        ```
       
    6. Удаление временной `/temp` директории:
       
        ```shell
        rm -r temp
        ```
       
    7. Commit новых файлов как в Develop, так и в Monolit Service.
    
## Актуализация кодовой базы сервиса

Добавление возможности пермаментной актуализации кодовой базы на примере Monolit Service

1. Добавление `volume` в сервис docker-compose.yml:

    ```yaml
      monolit-service:
        # ...
        volumes:
          - PATH_TO_SERVICE_DIR:/app
        # ...
    ```
    
    > Где PATH_TO_SERVICE_DIR - директория кодовой базы сервиса.

2. Перезапуск контейнера

    ```shell
    docker-compose up -d --force-recreate --no-deps monolit-service
    ```

    > Если сервис запущен не стандартной командой CMD - требуется перезапустить его соответствующим способом.
  
    
Для применения изменения кодовой базы требуется перезапуск всех слушателей сервиса.

