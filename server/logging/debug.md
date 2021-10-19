# Инструменты отладки

В данном разделе приводятся примеры использования всевозможных механизмов отладки работы микросервиса.

## Встроенные средства php

- var_dump()
- print_r()

## Средства Symfony

- dd()
- dump()

## Средства laravel

Laravel интегрирован с библиотекой [Monolog](https://github.com/Seldaek/monolog), 
которая обеспечивает поддержку множества мощных обработчиков логов.
Подробнее см. [Laravel Logging](https://laravel.com/docs/8.x/logging)

- Illuminate\Support\Facades\Log::info();
- Illuminate\Support\Facades\Log::debug();
- Illuminate\Support\Facades\Log::log();

## Примеры использования:

### Логирование в файл

Для этой цели можно воспользоваться средствами laravel.
Этот механизм позволяет писать отладочную информацию в файл логов, находящийся в директории микросервиса
`storage/logs` с названием `lumen-****-**-**.log`.

Как пример можно рассмотреть логирование в действиях.

```php
<?php

namespace App\Models;
use Illuminate\Support\Facades\Log;
use Egal\Model\Model as EgalModel;

class Order extends EgalModel
{
    public static function actionUpdate($id = null, array $attributes = []): array
    {
        Log::info('id: ' . $id);
        Log::info('attributes: ' . print_r($attributes, true));
        // ...
    }
}
```

После выполнения запроса на обновление, вывод будет примерно такой:

```text
[2021-06-07 06:42:50] local.INFO: id: 53  
[2021-06-07 06:42:50] local.INFO: attributes: Array
(
    [name] => Ivanov
)
```

### В консольных командах

Порой бывает необходимость "быстро" проверить на работоспособность часть кода.
Для этого достаточно удобно создать отладочную команду и делать это там.

Рассмотрим пример работы такой команды. Создадим команду по такому пути `app/Console/Commands/DebugCommand.php`.

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Facades\Log;

class DebugCommand extends Command
{

    protected $signature = 'debug';

    public function handle(): void
    {
        $country = new Country();
        $country->name = 'Russia';
        
        var_dump($country->name);
        print_r($country->name);
        dump($country->name);
        dd($country->name);
    }
}
```

Выполним команду для просмотра вывода
```shell
$ php artisan debug
Russia
string(6) "Russia"
^ "Russia"
^ "Russia"
```

### Отладка путем вывода информации в stdout демона.

Рассмотрим пример отладки работы `action` путем запуска слушателя в терминале.
Прописываем в `action` для отладки:

```php
// ...
public static function actionUpdate($id = null, array $attributes = []): array
{
        print_r('id: ' . $id);
        var_dump($attributes);
        // ...
}
// ...
```

Запускаем слушателя
```shell
docker-compose exec order-service ./artisan egal:listener:run
```

Теперь вся отладочная информация будет выводиться в терминале.

## Отладка PL/pgSQL функций в pgAdmin

В рамках фреймворка Egal поставляется 2 образа контейнера с СУБД PostgresSQL - с расширением для отладки и без него.
Для переключения на образ с отладчиком необходимо в docker-compose.yml для контейнера с БД поставить образ `2.1.0-pldbgapi`.
После этого появится возможность пошаговой отладки функций в pgAdmin. Подробное использование расширения для отладки можно изучить
в [официальной документации pdAdmin](https://www.pgadmin.org/docs/pgadmin4/4.29/debugger.html)