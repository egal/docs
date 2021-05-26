# Sentry

Инструмент разработчиков для отслеживания и исправления сбоев в режиме реального времени. Реализован с открытым исходным кодом.

Из соображений безопасности для проектов компании используется Self Hosted Sentry (для получения доступа обратитесь к команде Egal).

Существует облачное решение Sentry для использования в личных целях. 

## Как подключить

[Официальная документация](https://docs.sentry.io/platforms/).

## Как подключить в Egal

1. Зайти либо зарегистрироваться в Sentry.
2. Создать Организацию (если это требуется).
3. Создать проект (если это требуется).
4. В сервисе установить пакет Sentry для Egal приложения:

```bash
composer require sentry/sentry-laravel
```

5. Скопировать файл конфигурации из библиотеки в директорию конфигураций.

```bash
cp ./vendor/sentry/sentry-laravel/config/sentry.php ./config/sentry.php
```

6. Зарегистрировать `Sentry\Laravel\ServiceProvider` в файле
   `bootstrap/app.php`:

```php
$app->register(Sentry\Laravel\ServiceProvider::class);
```

8. Добавить `SENTRY_LARAVEL_DSN` (`DSN`) в `.env`:

```dotenv
SENTRY_LARAVEL_DSN=http://${SECURITY_TOKEN}@sentry.egal.smw.tom.ru/${PROJECT_ID}
```

> Доступные `DSN` хранятся в настройки проекта (путь `SDK
> Setup / Client Keys`).

9. Добавление Sentry Reporting в `App/Exceptions/Handler`:

```php
public function report(Exception $exception)
{
    if (app()->bound('sentry') && $this->shouldReport($exception)){
        app('sentry')->captureException($exception);
    }

    parent::report($exception);
}
```

10. Для проверки работоспособности - вызвать любой Exception
