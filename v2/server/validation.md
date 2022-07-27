# Валидация

Валидация выполняется при срабатывании события `saving`. В ситуациях, когда перед валидацией или после нее требуется
выполнить какие-то действия (например, системой установить значения атрибутам), можно использовать события
`validating`/`validated`. Если валидация происходит в результате выполнения `actionCreate`/`actionUpdate` например, 
срабатывают также события `validatingWithAction`/`validatedWithAction`.

## Список правил валидации

### upper_case

Атрибут введен в верхнем регистре.


### lower_case

Атрибут введен в нижнем регистре.


### Правила валидации Laravel

[Cм. документацию](https://laravel.com/docs/master/validation).


## Пользовательские правила валидации

> В проекте используется библиотека `egal/validation`.

1. Создайте файл с правилами командой:

   ```bash
   php artisan egal:make:rule ExampleRule
   ```

2. Реализуйте метод `validate`

   ```php
   namespace App\Rules;

   use Egal\Validation\Rules\Rule as EgalRule;

   class ExampleRule extends EgalRule
   {

       // ...

       public function validate($attribute, $value, $parameters = null): bool
       {
           // Твоя реализация метода
       }

       // ...

   }
   ```

Для использования правила валидации применяется
[вызов через класс](https://laravel.com/docs/master/validation#using-rule-objects)
либо через строку.

При определении правила через строку берется атрибут `rule` у класса.
Если он не указан берется название класса без постфикса `Rule` в snake
регистре (Класс - `App\Rules\UpperCaseRule`, строка определения правила
`upper_case`).

Чтобы задать текст ошибки валидации, переопределите метод `message`:

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

