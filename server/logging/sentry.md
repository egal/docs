# Sentry

Инструмент помогает разработчикам отслеживать и исправлять сбои в режиме
реального времени. Реализован с открытым исходным кодом.

Существует облачное решение Sentry и Self Hosted.


## Как подключить

См. [официальную документацию](https://docs.sentry.io/platforms/).


## Как подключить в Egal

1. Зайдите либо зарегистрируйтесь в Sentry.
2. Создайте Организацию (если это требуется).
3. Создайте проект (если это требуется).
4. В вашем сервисе установите пакет Sentry для вашего Egal приложения:

```bash
composer require sentry/sentry-laravel
```

5. Скопируйте файл конфигурации из библиотеки в директорию конфигураций.

```bash
cp ./vendor/sentry/sentry-laravel/config/sentry.php ./config/sentry.php
```

6. Зарегистрируйте `Sentry\Laravel\ServiceProvider` в файле
   `bootstrap/app.php`:

```php
$app->register(Sentry\Laravel\ServiceProvider::class);
```

8. Добавте свой `SENTRY_LARAVEL_DSN` (`DSN`) в `.env`:

```dotenv
SENTRY_LARAVEL_DSN=http://${SECURITY_TOKEN}@sentry.your-domain.com/${PROJECT_ID}
```

> Чтобы получить свой `DSN`, перейдите в настройки проекта по пути `SDK
> Setup / Client Keys`. Там выберите один из доступных DSN

9. Добавьте Sentry Reporting в `App/Exceptions/Handler`:

```php
public function report(Exception $exception)
{
    if (app()->bound('sentry') && $this->shouldReport($exception)){
        app('sentry')->captureException($exception);
    }

    parent::report($exception);
}
```

10. Проверьте работоспособность, вызвав любой Exception.

