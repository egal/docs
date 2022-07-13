# Быстрый старт

При необходимости реализовать набор REST API для модели требуется выполнить следующие шаги:
1. Создание политики для настройки доступов до rest-endpoints,
2. Формирование метаданных для валидации полей модели, 
3. Регистрация rest-маршрутов.

Рассмотрим подробнее каждый шаг на примере модели `Post`.
Для создания модели, наследованной от `Egal\Core\Database\Model` можно воспользоваться artisan командой:

```bash
php artisan egal:make:model Post
```

## Настройка доступов

Для генерации класса политики модели `Post` необходимо вызвать artisan команду:

```bash
php artisan egal:make:policy Post
```

По умолчанию доступ до всех rest-enpoints закрыт, для того, чтобы сделать endpoint общедоступным, необходимо 
в соответствующих методах политики возвращать значение `true`. Сделаем `show` и `index` общедоступными:

```php
class PostPolicy
{

    public function showAny(?User $user): bool
    {
        return true;
    }

    public function show(?User $user, Post $post): bool
    {
        return true;
    }

    ...
}
```

## Валидация полей

В классе модели `Post` имеется метод `initializeMetadata()` для формирования метаданных модели, с помощью которого
можно указать поля модели и их правила валидации. 

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

## Регистрация маршрутов

Для создания rest-маршрутов необходимо в файле маршрутов `api.php` вызвать `Egal\Core\Facades\Route::rest(Post::class, PostPolicy::class);`.

```php
<?php

use App\Http\Policies\PostPolicy;
use App\Models\Post;
use Egal\Core\Facades\Route as EgalRoute;


EgalRoute::rest(Post::class, PostPolicy::class);
```

В результате чего создаются следующие маршруты:

|  Метод   |    Адрес    | Endpoint | Назначение                   |
|:--------:|:-----------:|:--------:|:-----------------------------|
| GET/HEAD |    posts    |  index   | Получить список всех постов. |
|   POST   |    posts    |  create  | Создать пост.                |
| GET/HEAD | posts/{key} |   show   | Получить пост по ключу.      |
|  DELETE  | posts/{key} |  delete  | Удалить пост по ключу.       |
|  PATCH   | posts/{key} |  update  | Редактировать пост по ключу. |

>Для обращения к запросам на получение данных необходимо указание query-параметра select. Подробнее описано в разделе
> [Доступные query-параметры](/v3/server/rest#Доступные-query-параметры)
