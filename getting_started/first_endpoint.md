# Первый Endpoint

1. Разворачивание всего зависимого окружения (RabbitMQ, базы данных и т.д.)
2. Создайте сущность в сервисе, где будет endpoint

```bash
php artisan egal:make:model Example
```

После выполнения команды в директории `app\Models` создается
файл.

3. В сгенерированном файле описываются метаданные сущности (см. [документацию](/server/metadata.md))
4. Генерируется миграция

```bash
php artisan egal:make:migration-create Example
```

После выполнения команды в директории `database/migrations`
создается файл.

6. Запускается/перезапускается демон сервиса

```bash
php artisan egal:run
```

7. Вызывается нужный Endpoint с помощью Web Service, либо с помощью
   [сообщения в RabbitMQ](/concepts/request_to_server_lifecycle.md).

