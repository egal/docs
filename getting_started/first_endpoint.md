# Первый Endpoint

1. Разворачивание всего зависимого окружения (RabbitMQ, базы данных и т.д.)
2. Создайте сущность в сервисе, где будет endpoint

```bash
php artisan egal:make:model Example
```

После выполнения команды в директории `app\Models` создастся
файл.

3. Откройте сгенерированный файл модели и опишите метаданные сущности (см. [документацию](/server/metadata.md))
4. Сгенерируйте миграцию

```bash
php artisan egal:make:migration-create Example
```

После выполнения этой команды в директории `database/migrations`
создастся файл.

6. Сделайте запуск/перезапуск демона

```bash
php artisan egal:run
```

7. Вызовите нужный Endpoint с помощью Web Service, либо с помощью
   [сообщения в RabbitMQ](/concepts/request_to_server_lifecycle.md).

