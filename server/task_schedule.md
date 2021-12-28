# Task Scheduling

## Вступление

Подробнее об использовании планировщика задач смотри
[официальную документацию Laravel](https://laravel.com/docs/8.x/scheduling).

## Запуск в контейнере

С помощью Docker Compose есть возможность развернуть обработчик задач в отдельном контейнере. 
Рассмотрим на примере, в файле docker-compose.yml определим сервис `monolit-service` и отдельно планировщик задач этого сервиса. 
Для дублирования свойств воспользуемся якорями:

```yaml
version: "3.7"

x-monolit-service: &monolit-service
  build: server/monolit-service
  restart: always
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

services:
  monolit-service:
    <<: *monolit-service
    command: bash -c "/wait && php artisan egal:listener:run"
  monolit-service-schedule:
    <<: *monolit-service
    command: bash -c "/wait && php artisan schedule:work"
```

Таким же образом можно добавить сервис для обработчика очередей, например.
