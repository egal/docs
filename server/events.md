# События и реакции

## Публикация событий

Публикация локальных событий происходит средствами Lumen.

> Для создания файлов событий используйте
> [генераторы](#Генерация-файла-события).

### Публикация событий в RabbitMQ

Для публикации в RabbitMQ стандартных событий модели как глобальных используйте
`$dispatchesEvents` и `ModelGlobalEvents`:

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

### Публикация событий в Centrifugo
Для реализации событий* с публикацией в Centrifugo предоставляется 2 варианта:
1) Наследоваться от CentrifugoEvent
2) Подключить трейт CentrifugoPublishable

* Для публикации в Centrifugo стандартных событий модели как глобальных использовать $dispatchesEvents и ModelCentrifugoEvents
```
protected $dispatchesEvents = [
        'saved' => SavedModelCentrifugoEvent::class,
        'saving' => SavingModelCentrifugoEvent::class,
        'retrieved.action' => RetrievedModelCentrifugoEvent::class
    ];
```

Формирование списка каналов для публикации:
```
1) $service,                                                     // все события сервиса, всегда присутствует
2) $service . '@' . $event                                       // все определенные события сервиса, всегда присутствует (Например, все UpdatedModelCentrifugoEvent)
3) $service . '@' . $modelName . '.' . $event,                   // все определенные события одной модели сервиса, присутствует, когда в событии есть модель (Например, все UpdatedModelCentrifugoEvent для модели User)
4) $service . '@' . $modelName,                                  // все события одной модели сервиса, присутствует, когда в событии есть модель (Например, все события для модели User)
5) $service . '@' . $modelName . '.' . $modelId . '.' . $event,  // все определенные события одного объекта одной модели сервиса, присутствует, когда в событии есть модель с id (Например, все UpdatedModelCentrifugoEvent для модели User с id=1)
6) $service . '@' . $modelName . '.' . $modelId,                 // все события одного объекта модели сервиса, присутствует, когда в событии есть модель с id (Например, все события для модели User с id=1)
```

Формирование сообщения:
1) Когда в событии есть модель:
```
[
                'type' => 'model_event',
                'data' => [
                    'name' => 'updated_model',
                    'model_name' => 'User',
                    'model_id' => 1,
                ],
]
```
2) Когда в событии нет модели:
```
 [
                'type' => 'event',
                'data' => [
                    'name' => 'test_centrifugo_event',
                ],
]
```

## Определение обработчиков событий

Обработчики определяются в файле `app/Providers/EventServiceProvider`.
Если его нет, то необходимо
[сгенерировать](#Генерация-файла-eventserviceprovider) его.

Определим локальные события и их обработчики:

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

Определим паттерны глобальных событий и их обработчики:

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

При возникновении события вызывается метод `handle()` у обработчика
события. Всю логику обработки события нужно разместить в этом методе.

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

> Для создания файлов обработчиков событий используйте
> [генераторы](#Генерация-файла-обработчика-события).


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

После выполнения этой команды в директории `app/Events` сгенерируется
файл события.

> Глобальные события публикуются на все приложение (не только в рамках
> сервиса). Обработка глобального события может происходить не в текущем
> клоне сервиса.


### Генерация файла обработчика события

Консольная команда:

```bash
php artisan egal:make:listener Example
```

Команда для генерации файла обработчика глобольного события:

```bash
php artisan egal:make:listener Example --global
```

После выполнения этой команды в директории `app/Events` сгенерируется
файл события.


### Список поддерживаемых событий

|       Событие       | В какой момент наступает событие       |
|:-------------------:|:---------------------------------------|
|      retrieved      | После получения данных.                |
|       saving        | В момент сохранения модели.            |
|        saved        | После сохранения.                      |
|      creating       | В момент создания.                     |
|       created       | После создания.                        |
|      updating       | В момент обновления.                   |
|       updated       | После обновления.                      |
|      deleting       | В момент удаления.                     |
|       deleted       | После удаления.                        |
| retrievedWithAction | После получения данных в action.       |
|  savingWithAction   | В момент сохранения в модели в action. |
|   savedWithAction   | После сохранения в action.             |
| creatingWithAction  | В момент создания модели в action.     |
|  createdWithAction  | После создания в action.               |
| updatingWithAction  | В момент обновления модели в action.   |
|  updatedWithAction  | После обновления модели в action.      |
| deletingWithAction  | После получения данных в action.       |
|  deletedWithAction  | После получения данных в action.       |

