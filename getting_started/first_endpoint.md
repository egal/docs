# Первый Endpoint

1. Разверните зависимое окружение (RabbitMQ, базы данных и т.д.)
2. Зайдите в сервис в котором желаете создать первый endpoint
3. Создайте сущность

```bash
php artisan egal:make:model Example
```

После выполнения команды в директории `app\Models` создастся
файл.

3. Откройте сгенерированный файл модели
4. Опишите метаданные сущности (см. [документацию](/server/metadata.md))
5. Сгенерируйте миграцию

```bash
php artisan egal:make:migration-create Example
```

После выполнения команды в директории `database/migrations`
создастся файл.

6. Сделайте запуск/перезапуск демона сервиса

```bash
php artisan egal:run
```

7. Вызовите нужный Endpoint либо с помощью Web Service, либо с 
   [сообщения в RabbitMQ](/architecture_concepts/request_to_server_lifecycle.md).

