# Конфигурация

В данном разделе рассматривается конфигурация сервисов и основных
контейнеров с описанием основных переменных окружения.


## Виды конфигурации

1. Конфигурация сервиса.
2. Конфигурация проекта.


### 1. Конфигурация сервиса

Конфигурация сервиса задается через переменные окружения, которые
расположены:
* в файле .env в корне сервиса
* в файле docker-compose.yml в разделе `enviroment` сервиса.

Пример конфигурирования сервиса через docker-compose.yml файл.

```yaml
...
interface-service:
  container_name: ${PROJECT_NAME}-interface-service
  image: egalbox/interface-service:tagname
  volumes:
    - /home/user/project/${PROJECT_NAME}/interface-service:/app
  environment:
    WAIT_HOSTS: ${PROJECT_NAME}-rabbitmq:5672, ${PROJECT_NAME}-database:5432
    APP_SERVICE_NAME: interface
    APP_SERVICE_KEY: mIcWu@Fqbj1mUK7h#JQTtYaXpSz192ed
    APP_NAME: interface
    APP_ENV: local
    DB_CONNECTION: pgsql
    DB_HOST: ${PROJECT_NAME}-database
    DB_PORT: ${DB_DATABASE:-5432}
    DB_DATABASE: ${DB_DATABASE:-interface}
    DB_USERNAME: ${DB_USERNAME:-postgres}
    DB_PASSWORD: ${DB_PASSWORD:-password}
    RABBITMQ_HOST: ${PROJECT_NAME}-rabbitmq
    RABBITMQ_PORT: ${RABBITMQ_PORT:-5672}
    RABBITMQ_USER: ${RABBITMQ_USERNAME:-admin}
    RABBITMQ_PASSWORD: ${RABBITMQ_PASSWORD:-password}
  command: ./artisan egal:run
...
```

В данном случае нас интересует раздел `environment`. В нем находятся
переменные окружения, которые передаются в контейнер сервиса. Таким
образом мы можем настроить само приложение которое находится в
контейнере.

При обычном подходе переменные находятся в файле `.env` в корне сервиса.
В данном случае вы работаете точно так же, как бы вы работали в типичном
проекте на lumen или laravel. Подробнее можно посмотреть здесь
[Lumen Configuration](https://lumen.laravel.com/docs/8.x/configuration).

Все остальные разделы (container_name, image, volumes, command и т.д.)
позволяют настроить контейнер нашего сервиса.
[Docker Compose](https://docs.docker.com/compose/)

> Конструкция вида `${DB_DATABASE:-5432}` означает, что подставляется
> переменная окружения DB_DATABASE, либо значение по умолчанию `5432`.

> Если значение переменной (в том числе значение по умолчанию) не
> определено, используется пустое значение.


### Переменные окружения

| Название переменной         | Описание                                                                                                 | Возможные значения                           |
|:----------------------------|:---------------------------------------------------------------------------------------------------------|:---------------------------------------------|
| PROJECT_NAME                | Название проекта.                                                                                        | Строка                                       |
| APP_SERVICE_NAME            | Название сервиса.                                                                                        | Строка                                       |
| APP_SERVICE_KEY             | Ключ, используемый для подписи UMT (User Master Token).                                                  | Строка                                       |
| APP_SERVICES                | Все сервисы проекта и их ключи для регистрации в сервисе авторизаци.                                     | Строка                                       |
| WAIT_HOSTS                  | Перечень хостов, перечисленных через запятую, которых должен ждать текущий контейнер, чтобы загрузиться. | Хост вида: project-rabbitmq:5672             |
| APP_NAME                    | Название приложения, см. [Lumen Configuration](https://lumen.laravel.com/docs/8.x/configuration)         | Строка                                       |
| APP_ENV                     | Определяет рабочую среду см. [Lumen Configuration](https://lumen.laravel.com/docs/8.x/configuration)     | local, dev, development, staging, production |
| APP_DEBUG                   | Режим отладки см. [Lumen Configuration](https://lumen.laravel.com/docs/8.x/configuration)                | true, false                                  |
| RABBITMQ_HOST               | Содержит Host, где работает RabbitMQ.                                                                    | Строка                                       |
| RABBITMQ_PORT               | Содержит порт на котором работает RabbitMQ.                                                              | Целое неотрицательное число                  |
| RABBITMQ_USER               | Пользователь для подключения к RabbitMQ.                                                                 | Строка                                       |
| RABBITMQ_PASSWORD           | Пароль для подключения к RabbitMQ.                                                                       | Строка                                       |
| AUTH_USER_MASTER_TOKEN_TTL  | Время жизни мастер токена в секундах.                                                                    | Целое неотрицательное число                  |
| AUTH_USER_SERVICE_TOKEN_TTL | Время жизни сервис токена в секундах.                                                                    | Целое неотрицательное число                  |
| DISABLE_AUTH                | Параметр отключающий авторизацию на сервисе.                                                             | 1, 0                                         |
| DB_CONNECTION               | Тип драйвера для подключения к базе данных.                                                              | pgsql, mysql, sqlite                         |
| DB_HOST                     | Хост для подключения к базе данных.                                                                      | Строка                                       |
| DB_PORT                     | Порт подключения к базе данных.                                                                          | Целое неотрицательное число                  |
| DB_DATABASE                 | Название БД для сервиса сервис.                                                                          | Строка                                       |
| DB_USERNAME                 | Пользователь для подключения к базе данных.                                                              | Строка                                       |
| DB_PASSWORD                 | Пароль для подключения к базе данных.                                                                    | Строка                                       |


### 2. Конфигурация проекта

Конфигурация проекта - совокупность конфигураций контейнеров. Содержит
конфигурации основных сервисов и сервисов, обеспечивающих их работу. К
последним относятся:
- rabbitmq
- database

```yaml
...
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

  database:
    container_name: ${PROJECT_NAME}-database
    image: egalbox/postgres:tagname
    restart: always
    ports:
      - ${DATABASE_PORT:-5434}:5432
    volumes:
      - ./database/data:/var/lib/postgresql/data
    environment:
      POSTGRES_MULTIPLE_DATABASES: interface,monolit,auth
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${DATABASE_PASSWORD:-password}

...
```

Где переменные окружения

| Название переменной         | Описание                                                    | Возможные значения |
|:----------------------------|:------------------------------------------------------------|:-------------------|
| RABBITMQ_USERNAME           | Имя пользователя для подключения к RabbitMQ.                | Строка             |
| RABBITMQ_PASSWORD           | Пароль для подключения к RabbitMQ.                          | Строка             |
| RABBITMQ_PLUGINS            | Активируемые плагины RabbitMQ. Перечисляются через запятую. | Строка             |
| POSTGRES_MULTIPLE_DATABASES | Список баз данных для других сервисов в проекте.            | Строка             |
| POSTGRES_USER               | Имя пользователя для подключения к базе данных.             | Строка             |
| POSTGRES_PASSWORD           | Пароль для подключения к базе данных.                       | Строка             |


Часть переменных выносится в .env файл в корне проекта. Он используется
приложением docker-compose, а переменные подставляются в файле
docker-compose.yml при build и запуске контейнеров.

| Название переменной   | Описание                                                             | Возможные значения          |
|:----------------------|:---------------------------------------------------------------------|:----------------------------|
| PROJECT_NAME          | Название проекта.                                                    | Строка                      |
| RABBITMQ_PORT         | Пароль для подключения к RabbitMQ.                                   | Целое неотрицательное число |
| RABBITMQ_MANAGER_PORT | Активируемые плагины RabbitMQ. Перечисляются через запятую.          | Целое неотрицательное число |
| DATABASE_PORT         | Список баз данных, которые используются в проекте другими сервисами. | Целое неотрицательное число |
| DATABASE_PASSWORD     | Имя пользователя для подключения к базе данных.                      | Строка                      |

