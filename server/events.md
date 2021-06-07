# События и реакции

## Публикация событий

Публикация локальных событий происходит средствами Lumen.

> Для генерации файлов событий используйте [генераторы](#Генерация-файла-события).

Для публикации стандартных событий модели как глобальные используйте `$dispatchesEvents` и `ModelGlobalEvents`:
```php
namespace App\Models;

use Egal\Model\Events\SavedModelGlobalEvent;
use Egal\Model\Events\SavingModelGlobalEvent;
use Egal\Model\Events\RetrievedModelGlobalEvent;

class ExampleModel extends EgalModel {

    // ...

    protected $dispatchesEvents = [
        'saved' => SavedModelGlobalEvent::class,
        'saving' => SavingModelGlobalEvent::class,
        'retrieved.action' => RetrievedModelGlobalEvent::class
    ];

    // ...

}
```

## Определение обработчиков событий

Для определения обработчиков нам потребуется файл `app/Providers/EventServiceProvider`. Если данного файла не обнаружено - [сгенерируйте его](#Генерация-файла-eventserviceprovider).

В следующем блоке определяются локальные события и их обработчики:
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

В следующем блоке определяются паттерны глобальных событий и их обработчики:
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

При возникновении события вызывается метод `handle()` у обработчика события. Всю логику обработки события нужно разместить в данном методе.
```php
namespace App\Listeners;

use Egal\Core\Listeners\GlobalEventListener;
use Illuminate\Support\Facades\Log;

class ExampleListener extends GlobalEventListener
{

    public function handle(array $data): void
    {
        // Твоя реализация метода
    }

}
```

> Для генерации файлов обработчиков событий используйте [генераторы](#Генерация-файла-обработчика-события).


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
Или эта команда, для генерации файла глобольного события:
```bash
php artisan egal:make:event Example --global
```
После выполнения данной команды в директории `app/Events` сгенерируется файл события.

> Глобальные события публикуются на всё приложение (не только в рамках сервиса).
> А также обработка глобального события может происходить не в текущем клоне сервиса.

### Генерация файла обработчика события

Консольная команда:
```bash
php artisan egal:make:listener Example
```
Или эта команда, для генерации файла обработчика глобольного события:
```bash
php artisan egal:make:listener Example --global
```
После выполнения данной команды в директории `app/Events` сгенерируется файл события.

### Список поддерживаемых событий

|      Событие        |     В какой момент наступает событие   |
|:-------------------:|:---------------------------------------|
| retrieved           | После получения данных.                |
| saving              | В момент сохранения модели.            |
| saved               | После сохранения.                      |
| creating            | В момент создания.                     |
| created             | После создания.                        |
| updating            | В момент обновления.                   |
| updated             | После обновления.                      |
| deleting            | В момент удаления.                     |
| deleted             | После удаления.                        |
| retrievedWithAction | После получения данных в action.       |
| savingWithAction    | В момент сохранения в модели в action. |
| savedWithAction     | После сохранения в action.             |
| creatingWithAction  | В момент создания модели в action.     |
| createdWithAction   | После создания в action.               |
| updatingWithAction  | В момент обновления модели в action.   |
| updatedWithAction   | После обновления модели в action.      |
| deletingWithAction  | После получения данных в action.       |
| deletedWithAction   | После получения данных в action.       |
