# Валидация

<!-- В Метадате используется валидация Laravel. Нехватает краткого описания, что здесь за валидация-->
## Список правил валидации

### upper_case

Атрибут введен в верхнем регистре.

### lower_case

Атрибут введен в нижнем регистре.

### Правила валидации Laravel

[Cм. документацию](https://laravel.com/docs/master/validation).


## Пользовательские правила валидации

> В проекте исползуется библиотека `egal/validation`.

1. Создание файла с правилами:

```bash
php artisan egal:make:rule ExampleRule
```

2. Реализация метода `validate`

```php
namespace App\Rules;

use Egal\Validation\Rules\Rule as EgalRule;

class ExampleRule extends EgalRule
{

    // ...

    public function validate($attribute, $value, $parameters = null): bool
    {
        // Реализация метода
    }

    // ...

}
```

Для использования правила валидации применяется
[вызов через класс](https://laravel.com/docs/master/validation#using-rule-objects)
либо через строку.

При определении правила через строку берется атрибут `rule` у класса.
Если он не указан берется название класса без постфикса `Rule` в
snake регистре (Класс - `App\Rules\UpperCaseRule`, строка определения
правила - `upper_case`).

Чтобы задать текст ошибки валидации необходимо переопределить метод `message`:

```php
namespace App\Rules;

use Egal\Validation\Rules\Rule as EgalRule;

class ExampleRule extends EgalRule
{

    // ...

    public function message()
    {
        // Реализация метода
    }

    // ...

}
```

