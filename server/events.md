# События и реакции

## Публикация событий

Публикация локальных событий происходит средствами Lumen.

> Для создания файлов событий используются [генераторы](#Генерация-файла-события).

Для публикации стандартных событий модели как глобальных используется `$dispatchesEvents` и `ModelGlobalEvents`:
```php
namespace App\Models;

use Egal\Model\Events\SavedModelGlobalEvent;
use Egal\Model\Events\SavingModelGlobalEvent;

class ExampleModel extends EgalModel {

    // ...

    protected $dispatchesEvents = [
        'saved' => SavedModelGlobalEvent::class,
        'saving' => SavingModelGlobalEvent::class
    ];

    // ...

}
```

## Определение обработчиков событий

Обработчики определяются в файле `app/Providers/EventServiceProvider`. Если его нет - [сгенерируйте](#Генерация-файла-eventserviceprovider).

Определение локальных событий и их обработчиков:
```php
namespace App\Providers;

use Egal\Core\Events\EventServiceProvider as ServiceProvider;
use App\Listeners\ExampleListener;
use App\Event\ExampleEventr;

class EventServiceProvider extends ServiceProvider
{

    // ...

    protected $listen = [
        ExampleEvent::class => [
            ExampleListener::class
        ]
    ];

    // ...

}
```

Определение паттернов глобальных событий и их обработчики:
```php
namespace App\Providers;

use Egal\Core\Events\EventServiceProvider as ServiceProvider;
use App\Listeners\ExampleGlobalListener;

class EventServiceProvider extends ServiceProvider
{

    // ...

    public array $globalListen = [
        'service' => [
            'Model' => [
                'event-message' => [
                    ExampleGlobalListener::class
                ]
            ]
        ]
    ];

    // ...

}
```

## Обработка событий

При возникновении события вызывается метод `handle()` у его обработчика. Вся логика обработки событий размещается в этом методе.
```php
namespace App\Listeners;

use Egal\Core\Listeners\GlobalEventListener;
use Illuminate\Support\Facades\Log;

class ExampleListener extends GlobalEventListener
{

    public function handle(array $data): void
    {
        // Реализация метода
    }

}
```

> Для создания файлов обработчиков событий используйте [генераторы](#Генерация-файла-обработчика-события).


## Генераторы

### Генерация файла EventServiceProvider

Консольная команда:
```bash
php artisan egal:make:event-service-provider
```

### Генерация файла события

Консольная команда:
```bash
php artisan egal:make:event Example
```
Команда для генерации файла глобального события:
```bash
php artisan egal:make:event Example --global
```
После выполнения этой команды в директории `app/Events` сгенерируется файл события.

> Глобальные события публикуются для всего приложения (не только в рамках сервиса).
> Обработка глобального события может происходить не в текущем клоне сервиса.

### Генерация файла обработчика события

Консольная команда:
```bash
php artisan egal:make:listener Example
```
Команда для генерации файла обработчика глобольного события:
```bash
php artisan egal:make:listener Example --global
```
После выполнения этой команды в директории `app/Events` сгенерируется файл события.
