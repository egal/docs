## Select

Определяется тэгом:
```vue
<e-select></e-select>
```

### Параметры
Селект принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр                   |       Тип       | По умолчанию | Возможные значения                                                                           | Описание                                                              |
|----------------------------|:---------------:|:------------:|----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| ``paceholder``             |     String      |    ``''``    | Любая строка                                                                                 | Placeholder                                                           |
| ``label``                  |     String      |    ``''``    | Любая строка                                                                                 | Label                                                                 |
| ``helperText``             |     String      |    ``''``    | Любая строка                                                                                 | Мелкий текст под селектом                                             |
| ``error``                  |     String      |    ``''``    | Любая строка                                                                                 | Вывод кастомной ошибки                                                |
| ``modelValue``             | [Object, Array] |    ``[]``    | Любой массив или объект с выбранным(и) из списка значением(ями) (можно использовать v-model) | Значение(я) селекта (можно использовать v-model)                      |
| ``options``                |      Array      |    ``[]``    | Массив объектов возможных значений                                                           | Список возможных значений                                             |
| ``shownKey``               |     String      |   ``name``   | Любая строка                                                                                 | Ключ, по которому выводить значение в селект и дропдаун               |
| ``validators``             |      Array      |    ``[]``    | Массив функций-валидаторов                                                                   | Проверка значения будет проходить через каждую функцию-валидатор      |
| ``size``                   |     String      |    ``md``    | ``sm``, ``md``, ``lg``                                                                       | Размер селекта                                                        |
| ``showError``              |     Boolean     |   ``true``   | ``true``, ``false``                                                                          | Показ ошибки и состояния селекта                                      |
| ``clearable``              |     Boolean     |   ``true``   | ``true``, ``false``                                                                          | Возможность удаления введенного значения с помощью кнопки             |
| ``multiple``               |     Boolean     |  ``false``   | ``true``, ``false``                                                                          | Возможность выбора более одного значения из списка                    |
| ``searchable``             |     Boolean     |  ``false``   | ``true``, ``false``                                                                          | Осуществление поиска по списку значений                               |
| ``searchableInput``        |     Boolean     |  ``false``   | ``true``, ``false``                                                                          | Отображение инпута с поиском по дропдауну                             |
| ``searchPlaceholder``      |     String      |  ``Search``  | Любая строка                                                                                 | Для изменения дефолтного текста в инпуте с поиском в дропдауне        |
| ``emptyDropdownText``      |     String      | ``no data``  | Любая строка                                                                                 | Для текста если в дропдауне нет опций или ничего не найдено по поиску |
| ``grouped``                |     Boolean     |  ``false``   | ``true``, ``false``                                                                          | Группировка списока в дропдауне (нужен определенный вид options)      |
| ``dropdownPosition``       |     String      |  ``bottom``  | ``bottom``, ``top``                                                                          | Установка позиционирования списка опций                               |
| ``dropdownStyleConfig``    |     Object      |    ``{}``    | Объект стилей                                                                                | Кастомизация стилей дропдауна                                         |
| ``inputSearchStyleConfig`` |     Object      |    ``{}``    | Объект стилей                                                                                | Кастомизация стилей инпута поиска                                     |

2. объект `styleConfig`:
Набор стилей для кастомизация селекта.

### События
| Название              | Тип параметров | Описание                             |
|-----------------------|:--------------:|--------------------------------------|
| ``error``             |     String     | Возвращает ошибку из валидаторов     |
| ``update:modelValue`` |     String     | Возвращает выбранное(ые) значение(я) |

#### Пример options
```javascript
options = [
    {
        name: 'Option one',
        key: 1
    },
    {
        name: 'Option two',
        key: 2
    },
    {
        name: 'Option three',
        key: 3
    }
]
```


#### Пример groupedOptions
```javascript
groupedOptions = [
    {
        groupName: 'First group',
        options: [
            {
                name: 'Option one',
                key: 1
            },
            {
                name: 'Option two',
                key: 2
            },
            {
                name: 'Option three',
                key: 3
            }
        ]
    },
    {
        groupName: 'Second group',
        options: [
            {
                name: 'Option four',
                key: 4
            },
            {
                name: 'Option five',
                key: 5
            }
        ]
    }
]
```

#### Пример styleConfig
````javascript
styleConfig = {
        fontFamily: 'Open Sans',
        valueColor: 'black',
        valueFontWeight: '500',
        placeholderColor: 'gray',
        placeholderFontSize: '12px',
        labelColor: 'black',
        labelFontWeight: '600',
        labelFontSize: '12px',
        helperTextColor: 'gray',
        helperTextFontWeight: '400',
        helperTextFontSize: '10px',
        borderColor: 'gray',
        borderRadius: '6px',
        backgroundColor: 'white',
        focusBorderColor: 'blue',
        filledBackgroundColor: 'yellow',
        filledFontColor: 'blue',
        crossColor: 'gray',
        arrowColor: 'gray',
        errorColor: 'red',
      }
````

#### Пример dropdownStyleConfig
````javascript
dropdownStyleConfig = {
        fontFamily: 'Open Sans',
        optionColor: 'black',
        optionHoverBackgroundColor: 'gray',
        optionFontWeight: 500,
        activeBackgroundColor: 'blue',
        activeOptionColor: 'white',
        groupNameColor: 'gray',
        groupNameFontWeight: 600,
        backgroundColor: 'white',
        borderColor: 'blue',
        borderRadius: '10px',
        boxShadow: '0px 0px 1px rgba(12, 26, 75, 0.24), 0px 3px 8px -1px rgba(50, 50, 71, 0.05)'
      }
````