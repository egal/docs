# Установка

## Введение

Создадим структуру репозиториев, которая поможет легко и быстро разворачивать Egal Server.

## Первые шаги

1. Создаем новый репозиторий (в дальнейшем - Develop)
   для хранения кодовой базы проекта.
2. Клонируем его в нужную нам директорию и открываем в IDE
3. Создаем файл `.env` с содержанием:

    ```dotenv
    PROJECT_NAME=project
    ```

4. Создаем файл `docker-compose.yml` с содержанием:

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

    Должны получить примерно такой:
    
    ```html
    Hello, it's project/web-service!
    ```
    
    > Если ответ не получен - нужно дождаться полного старта сервисов.
    > Если в течение 1 минуты сервисы не запустились — проверьте docker-compose logs.
    > Если проблема не решена — нужно обратиться к команде Egal.

## Добавление стандартных сервисов Egal

На примере `auth-service` попробуем дополнить приложение новым сервисом
Добавления нового сервиса в приложение на примере `auth-service`

1. Дополним `docker-compose.yml`, секцию `services` сервисами:

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
    
    Ожидается ответ, содержащий `action_result` или `action_error`.

## Добавление собственных сервисов

На примере `monolit-service` попробуем дополнить приложение новым сервисом

1. Инициализируем composer проекта:
    
    ```shell
    composer create-project egal/egal monolit-service
    ```
    
    > Если выводится ошибка `Could not find package egal/egal with stability stable.` - выполняется следующая команда:
    > ```shell
    > composer create-project egal/egal monolit-service [VERSION] --stability dev
    > ```
    > Где `VERSION` - необходимая вам неcтабильная версия.

2. Дополним `docker-compose.yml`, секцию `services` сервисами:

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

3. Запустим сервисы:

    ```shell
    docker-compose up -d
    ```

4. Проверим работоспособность `monolit-service`

    ```shell
    curl localhost:80/monolit/MODEL_NAME/ACTION_NAME
    ```
    
    > Где MODEL_NAME - название модели внутри Monolit Service.
     
    > Где ACTION_NAME - название action у MODEL_NAME внутри Monolit Service.

5. Преобразуем `monolit-service` директорию в git submodule Develop репозитория:

    1. Перемещаем кодовую базу во временную директорию:
       
        ```shell
        mv monolit-service/ temp/
        ```
       
    2. Делаем commit текущего репозитория без `/temp` директории.
       
       > В изменениях должно отображаться удаление директории `/monolit-service`

    3. Инициализируем git submodules, если не инициализированы:
       
        ```shell
        git submodule init
        ```
       
    4. Инициализируем monolit-service submodule:

        ```shell
        git submodule add [URL] monolit-service
        ```
        > Где URL - HTTP или SSH ссылка на Git репозиторий Monolit Service. Репозиторий должен быть не пустым.

        > Если вы используете IDE JetBrains PhpStorm - на данном этапе его лучше перезапустить,
        > чтобы заново прошла индексация проекта.

    5. Актуализацируем кодовую базу Monolit Service репозитория:
       
        ```shell
        rsync -r ./temp/ ./monolit-service/
        ```
       
    6. Удаляем временную `/temp` директорию:
       
        ```shell
        rm -r temp
        ```
       
    7. Делаем commit всех новых файлов как в Develop, так и в Monolit Service.
    
## Актуализация кодовой базы сервиса

На примере Monolit Service добавим возможность пермаментной актуализации кодовой базы

1. Добавим `volume` в сервис docker-compose.yml:

    ```yaml
      monolit-service:
        # ...
        volumes:
          - PATH_TO_SERVICE_DIR:/app
        # ...
    ```
    
    > Где PATH_TO_SERVICE_DIR - директория кодовой базы сервиса.

2. Перезапустим контейнер

    ```shell
    docker-compose up -d --force-recreate --no-deps monolit-service
    ```

    > Если сервис запущен не стандартной командой CMD - перезапустите его соответствующим способом текущей реализации запуска.
  
    
Для применения изменения кодовой базы требуется перезапустить всех слушателей сервиса.

