# Разворот образа RabbitMQ

[Docker Hub](https://hub.docker.com/r/egalbox/rabbitmq)

## Установка

1. Откройте файл `docker-compose.yml`.
2. В блоке сервисов добавьте блок rabbitmq.

## Конфигурация

Укажите image: `egalbox/rabbitmq`.

Настройте environment:
* RABBITMQ_USER - имя пользователя, под которым используется RabbitMQ.
* RABBITMQ_PASSWORD - пароль пользователя.
* RABBITMQ_PID_FILE - путь до PID файла.
* RABBITMQ_PLUGINS - список плагинов через запятую, которые будут установлены в RabbitMQ ([список доступных плагинов](https://www.rabbitmq.com/plugins.html)).

## Запуск

`docker-compose up -d`
