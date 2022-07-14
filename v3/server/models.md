# Модель

>При создании модели для rest-endpoints необходимо наследование от `Egal\Core\Database\Model`. Для создания можно 
>использовать команду генерации:
>```bash
>php artisan egal:make:model Post
>```

## Метаданные модели

Каждая модель, наследованная от `Egal\Core\Database\Model`, должна реализовывать метод `initializeMetadata()`,
формирующий метаданные модели. На основе метаданных работает валидация клиентских данных, передаваемых на rest-endpoints,
связанные с данной моделью.

Рассмотрим все возможности метаданных на примере модели `Post`.

```php
<?php

namespace App\Models;

use Carbon\Carbon;
use Egal\Core\Database\Metadata\DataType;
use Egal\Core\Database\Metadata\ScopeParam;
use Egal\Core\Database\Model;
use Egal\Core\Database\Metadata\Model as ModelMetadata;
use Egal\Core\Database\Metadata\Field as FieldMetadata;
use Egal\Core\Database\Metadata\Relation as RelationMetadata;
use Egal\Core\Database\Metadata\Scope as ScopeMetadata;
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\MorphMany;

class Post extends Model
{

    public function initializeMetadata(): ModelMetadata
    {
        return ModelMetadata::make(static::class)
            ->setKeyName('uuid')
            ->setKeyType(DataType::String)
            ->fields(
                FieldMetadata::make('title', DataType::String)
                    ->required()
                    ->validationRules(['string'])
                    ->fillable(),
                FieldMetadata::make('channel_id', DataType::Integer)
                    ->required()
                    ->validationRules(['exists:channels,id'])
                    ->fillable()
            )
            ->relations(
                RelationMetadata::make('channel', Channel::class)
            )
            ->scopes(
                ScopeMetadata::make('titleStartWithCharacter', [ScopeParam::make('character', DataType::String)])
            );
    }

    public function channel(): BelongsTo
    {
        return $this->belongsTo(Channel::class);
    }

    public function comments(): MorphMany
    {
        return $this->morphMany(Comment::class, 'commentable');
    }

    public function scopeCreatedAfterToday($query)
    {
        return $query->where('created_at', '>', Carbon::today()->toDateString());
    }

    public function scopeTitleStartWithCharacter($query, $character)
    {
        return $query->where('title', 'like', $character . '%');
    }

    public function scopeRandomOne($query)
    {
        return $query->inRandomOrder()->limit(1);
    }

}
```

### Key метаданные

С помощью метаданных можно указать тип и наименование ключа модели, используя методы `setKeyName()` и `setKeyType()`. По 
умолчанию в качестве ключа устанавливается поле id с типом integer.

>При указании типа и наименования ключа в метаданных не требуется повторно указывать это в классе модели.

### Fields метаданные

Метод `fields()` принимает метаданные всех полей модели. 

При создании метаданных поля обязательными параметрами являются 
наименование и тип. Помимо этого можно определить правила валидации поля и его свойство `fillable`. 

>При указании поля как `fillable` в метаданных не требуется повторно указывать это в классе модели.

### Relation метаданные 

Метод `relations()` принимает метаданные всех отношений модели.

При создании метаданных отношения обязательными параметрами являются
наименование и класс модели, связанной отношением с текущей.

### Scopes метаданные  

Метод `scopes()` принимает метаданные всех scopes модели.

При создании метаданных scope обязательными параметрами являются
наименование метода scope и массив входных параметров.

> Метаданные необходимо указывать для работы валидации полей, отношений и scopes, приходящих с клиента.