# Метаданные

## Описание метаданных модели

Метаданные описываются в phpDocs модели.

Пример:

```php
/**
 * @property int $id            {@property-type field}  {@primary-key}
 * @property string $email      {@property-type field}  {@validation-rules required|string|email|unique:users|email}
 * @property string $password   {@property-type field}  {@validation-rules required|string}
 * @property Carbon $created_at {@property-type field}
 * @property Carbon $updated_at {@property-type field}
 *
 * @property Collection $roles          {@property-type relation}
 * @property Collection $permissions    {@property-type relation}
 *
 * @action getItems     {@statuses-access guest}
 * @action getItem      {@statuses-access guest}
 * @action getMetadata  {@statuses-access guest}
 * @action create       {@statuses-access guest}
 * @action update       {@statuses-access guest}
 * @action delete       {@statuses-access guest}
 */
class User extends Model
{

}
```

### Перечень тегов

|         Наименование         | Описание                                                            |                       Возможные значения                       |
|:-------------------:|:--------------------------------------------------------------------|:--------------------------------------------------------------:|
|      @property      | Тег поля                   |                                                                |
|   @property-type    | Тег типа поля                                                  |                        field, relation                         |
|  @validation-rules  | Тег правил валидации поля (взяты из laravel)           | [Cм. документацию](https://laravel.com/docs/master/validation) |
|    @primary-key     | Тег первичного ключа                                            |                         Без параметров                         |
|       @action       | Тег endpoint-а                                                  |                                                                |
|  @statuses-access   | Тег доступа к действию в зависимости от статуса аутентификации |                         logged, guest                          |
|    @roles-access    | Тег ролевого доступа к действию                  |                 admin, задается пользователем                  |
| @permissions-access | Тег доступа к действию в зависимости от разрешений             |              authenticate, задается пользователем              |


## Метаданные модели и её действий

Для получения нужно обратиться к действию `getMetadata` у любой модели.

## Метаданные меню

Для получения нужно обратиться к сервису `interface` модели
`MenuMetadata` действию `getItem`.

> Для получения древовидной структуры пунктов меню добавьте `with`
> `entries_tree`.

> Сопоставление `InterfaceMetadata` и `MenuEntryMetadata`:
> `InterfaceMetadata.id` = `MenuEntryMetadata.interface_metadata_id`

Для добавления пункта меню обратиться к сервису `interface` модели
`MenuMetadata` действию `create`.

> Для добавления пункта меню в древовидную структуру ниже первого
> уровня указывайте атрибут `parent_menu_entry_metadata_id`, который
> указывает на родительский пункт меню.

## Метаданные интерфейсов

Для получения можно обратиться к сервису `interface` модели
`InterfaceMetadata` действию `getItem`.

Для добавления можно обратиться к сервису `interface` модели
`InterfaceMetadata` действию `create`.

> Для автоматической генерации интерфейса на клиенте предусмотрены
> автогенерируемые типы интерфейсов: `table`.

> При указывании типа, не поддерживающего автогенерацию на клиенте,
> валидация `data` будет пропускаться. При указывании типа,
> поддерживающего автогенерацию на клиенте, валидация `data` будет
> проходить в соответствии с требованиями типа к `data`.

