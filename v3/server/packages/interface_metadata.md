# Метаданные интерфейсов

Пакет метаданных интерфейсов предназначен для автогенерации стандартных компонентов и виджетов на клиенте, 
конфигурируемых метаданными. Данный пакет позволяет:
* Создавать метаданные компонентов и виджетов,
* Получать метаданные компонента по его наименованию с помощью HTTP-запроса.

Доступные для создания компоненты:
* Table

Доступные для создания виджеты:
* Checkbox
* Input
* Select

## Создание метаданных

По умолчанию метаданные формируются в корневой папке проекта по пути `/interfaces/components.php`. Путь можно изменить,
указав в параметрах окружения `INTERFACE_METADATA_FILE_PATH`.

Пример содержания подобного файла для конфигурирования таблиц `Сотрудники` и `Отделы`:

```php
<?php

use App\Models\Department;
use Egal\Core\Rest\Filter\Field;
use Egal\Core\Rest\Filter\FieldCondition;
use Egal\Core\Rest\Filter\Operator;
use Egal\Core\Rest\Filter\Query;
use Egal\Interface\Facades\Manager;
use Egal\Interface\Metadata\Components\Table\Computed\DateFormatComputed;
use Egal\Interface\Metadata\Components\Table\Computed\DecimalPlacesComputed;
use Egal\Interface\Metadata\Components\Table\Computed\FunctionComputed;
use Egal\Interface\Metadata\Components\Table\FieldType;
use Egal\Interface\Metadata\Components\Table\Table;
use Egal\Interface\Metadata\Components\Table\TableDataConfiguration;
use Egal\Interface\Metadata\Components\Table\TableField;
use Egal\Interface\Metadata\Components\Table\TableInterfaceConfiguration;
use Egal\Interface\Metadata\Widgets\Checkbox\Checkbox;
use Egal\Interface\Metadata\Widgets\Checkbox\CheckboxInterfaceConfiguration;
use Egal\Interface\Metadata\Widgets\Input\Input;
use Egal\Interface\Metadata\Widgets\Input\InputInterfaceConfiguration;
use Egal\Interface\Metadata\Widgets\Input\InputSize;
use Egal\Interface\Metadata\Widgets\Select\Select;
use Egal\Interface\Metadata\Widgets\Select\SelectDataConfiguration;
use Egal\Interface\Metadata\Widgets\Select\SelectInterfaceConfiguration;
use Egal\Interface\Metadata\Widgets\Select\SelectSize;

Manager::component(Table::make(
    'Сотрудники',
    TableInterfaceConfiguration::make(),
    TableDataConfiguration::make()
        ->setRequestModel(\App\Models\User::class)
        ->setRequestRelations(['departments'])
        ->setRequestFilter(
            Query::make([
                FieldCondition::make(Field::fromString('rate'), Operator::Equals, 2)
            ])
        )
        ->setRequestOrder(['full_name', 'desc'])
        ->setFields(
            TableField::make('ФИО', 'full_name', FieldType::String)
                ->setSortable(true)
                ->setFilterable(true)
                ->setSearchable(true)
                ->setComputed(FunctionComputed::make('toUpperCase()'))
                ->setEditWidget(
                    Input::make(
                        '',
                        InputInterfaceConfiguration::make()
                            ->setValidators(['required'])
                            ->setSize(InputSize::Lg))
                ),
            TableField::make('Дата рождения', 'birth_date', FieldType::Date)
                ->setSortable(true)
                ->setFilterable(true)
                ->setComputed(DateFormatComputed::make('dd-MM-yy'))
                ->setEditWidget(
                    Input::make(
                        '',
                        InputInterfaceConfiguration::make()
                            ->setValidators(['date'])
                            ->setSize(InputSize::Lg)
                    )
                ),
            TableField::make('Статус', 'is_active', FieldType::Boolean)
                ->setSortable(true)
                ->setEditWidget(Checkbox::make('', CheckboxInterfaceConfiguration::make())),
            TableField::make('Ставка', 'rate', FieldType::Numeric)
                ->setComputed(DecimalPlacesComputed::make(2))
                ->setEditWidget(
                    Input::make(
                        '',
                        InputInterfaceConfiguration::make()
                            ->setSize(InputSize::Lg)
                    )
                ),
            TableField::make('Отдел', 'departments.name', FieldType::ArrayOfString)
                ->setFilterable(true)
                ->setComputed(FunctionComputed::make('toLowerCase()'))
                ->setEditWidget(
                    Select::make(
                        '',
                        SelectInterfaceConfiguration::make()
                            ->setSize(SelectSize::Lg),
                        SelectDataConfiguration::make()
                            ->setRequestModel(Department::class)
                    )
                )
        )
));

Manager::component(Table::make(
    'Отделы',
    TableInterfaceConfiguration::make(),
    TableDataConfiguration::make()
        ->setRequestModel(\App\Models\Department::class)
        ->setFields(
            TableField::make('Наименование', 'name', FieldType::ArrayOfString)
                ->setFilterable(true)
                ->setComputed(FunctionComputed::make('toLowerCase()'))
                ->setEditWidget(
                    Select::make(
                        '',
                        SelectInterfaceConfiguration::make()
                            ->setSize(SelectSize::Lg),
                        SelectDataConfiguration::make()
                            ->setRequestModel(Department::class)
                    )
                )
        )
));
```

## Маршрут получения метаданных компонента

Для получения метаданных компонента по его наименованию используется следующий HTTP-запрос:

```http request
GET interface-metadata/{label}
```

Пример запроса:
```http request
curl --location --request GET 'http://localhost:80/interface-metadata/Сотрудники'
```

Полученный ответ:
```json
{
    "type": "Table",
    "label": "Сотрудники",
    "content": [],
    "data": {
        "fields": [
            {
                "label": "ФИО",
                "path": "full_name",
                "type": "string",
                "sortable": true,
                "searchable": true,
                "filterable": true,
                "computed": [],
                "editWidget": {
                    "type": "Input",
                    "label": "",
                    "content": {
                        "type": "test",
                        "placeholder": "",
                        "error": "",
                        "showSuccess": false,
                        "showFilled": true,
                        "disabled": false,
                        "validators": [
                            "required"
                        ],
                        "helperText": "",
                        "iconLeft": "",
                        "iconRight": "",
                        "size": "lg",
                        "showError": true,
                        "required": false,
                        "showArrows": true,
                        "clearable": true,
                        "postfix": "",
                        "labelDisabledColor": "",
                        "valueDisabledColor": "",
                        "helperDisabledColor": "",
                        "showSuccessIcon": false
                    },
                    "data": null
                }
            },
            {
                "label": "Дата рождения",
                "path": "birth_date",
                "type": "date",
                "sortable": true,
                "searchable": false,
                "filterable": true,
                "computed": [],
                "editWidget": {
                    "type": "Input",
                    "label": "",
                    "content": {
                        "type": "test",
                        "placeholder": "",
                        "error": "",
                        "showSuccess": false,
                        "showFilled": true,
                        "disabled": false,
                        "validators": [
                            "date"
                        ],
                        "helperText": "",
                        "iconLeft": "",
                        "iconRight": "",
                        "size": "lg",
                        "showError": true,
                        "required": false,
                        "showArrows": true,
                        "clearable": true,
                        "postfix": "",
                        "labelDisabledColor": "",
                        "valueDisabledColor": "",
                        "helperDisabledColor": "",
                        "showSuccessIcon": false
                    },
                    "data": null
                }
            },
            {
                "label": "Статус",
                "path": "is_active",
                "type": "boolean",
                "sortable": true,
                "searchable": false,
                "filterable": false,
                "computed": [],
                "editWidget": {
                    "type": "Checkbox",
                    "label": "",
                    "content": {
                        "checked": false,
                        "disabled": false,
                        "size": "md",
                        "checkboxRight": false,
                        "indeterminate": false
                    },
                    "data": null
                }
            },
            {
                "label": "Ставка",
                "path": "rate",
                "type": "numeric",
                "sortable": false,
                "searchable": false,
                "filterable": false,
                "computed": [],
                "editWidget": {
                    "type": "Input",
                    "label": "",
                    "content": {
                        "type": "test",
                        "placeholder": "",
                        "error": "",
                        "showSuccess": false,
                        "showFilled": true,
                        "disabled": false,
                        "validators": [],
                        "helperText": "",
                        "iconLeft": "",
                        "iconRight": "",
                        "size": "lg",
                        "showError": true,
                        "required": false,
                        "showArrows": true,
                        "clearable": true,
                        "postfix": "",
                        "labelDisabledColor": "",
                        "valueDisabledColor": "",
                        "helperDisabledColor": "",
                        "showSuccessIcon": false
                    },
                    "data": null
                }
            },
            {
                "label": "Отдел",
                "path": "departments.name",
                "type": "string[]",
                "sortable": false,
                "searchable": false,
                "filterable": true,
                "computed": [],
                "editWidget": {
                    "type": "Select",
                    "label": "",
                    "content": {
                        "placeholder": "",
                        "helperText": "",
                        "error": "",
                        "modelValue": [],
                        "options": [],
                        "shownKey": "name",
                        "validators": [],
                        "size": "lg",
                        "showError": true,
                        "clearable": true,
                        "multiple": false,
                        "searchable": false,
                        "searchableInput": false,
                        "searchPlaceholder": "Search",
                        "emptyDropdownText": "no data",
                        "grouped": false,
                        "isLocalOptions": true,
                        "showMoreButtonDisplay": false,
                        "closeDropdownAfterSelection": true,
                        "nonLocalOptionsTotalCount": 0,
                        "showMoreButtonText": "Show more...",
                        "dropdownStyleConfig": "[]",
                        "inputSearchStyleConfig": "[]",
                        "showFilled": false,
                        "shownBadgeKey": "",
                        "showSuccess": false,
                        "inputConfig": "[]",
                        "dropdownPosition": "bottom"
                    },
                    "data": {
                        "requestModel": "App\\Models\\Department"
                    }
                }
            }
        ],
        "requestModel": "App\\Models\\User",
        "requestRelations": [
            "departments"
        ],
        "requestFilter": {},
        "requestOrder": [
            "full_name",
            "desc"
        ]
    }
}
```