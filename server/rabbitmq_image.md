# Разворот образа RabbitMQ

Среди поставляемых фреймворком образов есть образ [сервера rabbitMQ](https://hub.docker.com/r/egalbox/rabbitmq).

## Установка
Откройте файл `docker-compose.yml`
В блоке сервисов добавьте блок rabbitmq.

## Конфигурирование
Укажите image: egalbox/rabbitmq.

Настройте environment:
- RABBITMQ_USER (установка имени пользователя под которым используется rabbitmq)
- RABBITMQ_PASSWORD (установка пароля)
- RABBITMQ_PID_FILE (указание места хранения файла с идентификационным номером процесса)
- RABBITMQ_PLUGINS (выбор [плагинов](https://www.rabbitmq.com/plugins.html))

## Запуск
`docker-compose up -d`
