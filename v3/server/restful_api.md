# RESTful API

При необходимости реализовать набор REST API для модели требуется выполнить следующие шаги:
1. Создание политики для настройки доступов до rest-ендпоинтов,
2. Формирование метаданных для валидации атрибутов модели, 
3. Регистрация rest-маршрутов.

Рассмотрим подробнее каждый шаг на примере модели `Post`.
Для создания модели, наследованной от `Egal\Core\Database\Model` можно воспользоваться `artisan` командой:

```bash
php artisan egal:make:model Post
```

#### Настройка доступов

Для генерации класса политики модели `Post` необходимо вызвать `artisan` команду:

```bash
php artisan egal:make:policy Post
```

По умолчанию доступ до всех rest-ендпоинтов закрыт, для того, чтобы сделать ендпоинт общедоступным, необходимо 
в соответствующих методах политики возвращать значение `true`. Сделаем ендпоинт `show` общедоступным:

```php
class PostPolicy
{

    public function showAny(?User $user): bool
    {
        // делает ендпоинт show общедоступным
        return true;
    }

    public function createAny(?User $user): bool
    {
        return false;
    }

    ...
}
```

#### Валидация атрибутов

В классе модели `Post` имеется метод `initializeMetadata()` для формирования метаданных модели, с помощью которого
можно указать атрибуты модели и их правила валидации. 

```php
<?php

namespace App\Models;

use Egal\Core\Database\Model;
use Egal\Core\Database\Metadata\Model as ModelMetadata;
use Egal\Core\Database\Metadata\Field as FieldMetadata;

class Post extends Model
{

    public function initializeMetadata(): ModelMetadata
    {
        return ModelMetadata::make(static::class)
            ->fields(
                FieldMetadata::make('title')
                    ->required()
                    ->validationRules(['string|unique:posts,title'])
                    ->fillable(),
                FieldMetadata::make('content')
                    ->required()
                    ->validationRules(['string'])
                    ->fillable()
            );
    }

}

```

#### Регистрация маршрутов

Для создания rest-маршрутов необходимо в файле маршрутов вызвать `Egal\Core\Facades\Route::rest(Post::class, PostPolicy::class);`. 
В результате чего создаются следующие маршруты:


|  Метод   |    Адрес    | Назначение                                    |
|:--------:|:-----------:|:----------------------------------------------|
| GET/HEAD |    users    | Получить список всех пользователей.           |
|   POST   |    users    | Создать пользователя.                         |
| GET/HEAD | users/{key} | Получить пользователя по идентификатору.      |
|  DELETE  | users/{key} | Удалить пользователя по идентификатору.       |
|  PATCH   | users/{key} | Редактировать пользователя по идентификатору. |

#### Query-параметры

#### Формат ответов
