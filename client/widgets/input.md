## Input

Определяется тэгом:
```vue
<e-input></e-input>
```

### Параметры
Input принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр           |       Тип        |  По умолчанию  | Возможные значения                                                                      | Описание                                                         |
|--------------------|:----------------:|:--------------:|-----------------------------------------------------------------------------------------|------------------------------------------------------------------|
| ``id``             |      String      | ``input-text`` | Любая строка                                                                            | Используется для связки label и input                            |
| ``type``           |      String      |    ``text``    | ``text``, ``number``, ``password``, ``search``                                          | Тип инпута                                                       |
| ``paceholder``     |      String      |     ``''``     | Любая строка                                                                            | Placeholder                                                      |
| ``label``          |      String      |     ``''``     | Любая строка                                                                            | Label                                                            |
| ``error``          |      String      |     ``''``     | Любая строка                                                                            | Вывод кастомной ошибки                                           |
| ``showSuccess``    |     Boolean      |   ``false``    | ``true``, ``false``                                                                     | Показывать "успешное" состояние инпута                           |
| ``showFilled``     |     Boolean      |    ``true``    | ``true``, ``false``                                                                     | Показывать "заполненное" состояние инпута                        |
| ``modelValue``     | [String, Number] |    ``null``    | Любая строка и число                                                                    | Значение инпута (можно использовать v-model)                     |
| ``disabled``       |     Boolean      |   ``false``    | ``true``, ``false``                                                                     | Заблокировать инпут                                              |
| ``validators``     |      Array       |     ``[]``     | Массив функций-валидаторов                                                              | Проверка значения будет проходить через каждую функцию-валидатор |
| ``helperText``     |      String      |     ``''``     | Любая строка                                                                            | Мелкий текст под инпутом                                         |
| ``iconLeft``       |      String      |     ``''``     | Любое имя иконки, которое есть в Bootstrap (``circle-fill``, ``chevron-right``, и т.д.) | Иконка слева                                                     |
| ``iconRight``      |      String      |     ``''``     | Любое имя иконки, которое есть в Bootstrap (``circle-fill``, ``chevron-right``, и т.д.) | Иконка справа                                                    |
| `size`             |      String      |      `md`      | `xs`, `sm`, `md`, `lg`                                                                  | Размер инпута                                                    |
| ``showError``      |     Boolean      |    ``true``    | ``true``, ``false``                                                                     | Показывать ошибку и состояние инпута                             |
| ``required``       |     Boolean      |   ``false``    | ``true``, ``false``                                                                     | Показывать звездочку обязательного инпута                        |
| ``showArrows``     |     Boolean      |    ``true``    | ``true``, ``false``                                                                     | Показывать стрелки для изменения значения в инпуте типа number   |
| ``min``            |      Number      | ``undefined``  | Любое число                                                                             | Минимальное значение в инпуте типа number                        |
| ``max``            |      Number      | ``undefined``  | Любое число                                                                             | Максимальное значение в инпуте типа number                       |
| ``inputMaxLength`` |      Number      | ``undefined``  | Любое число                                                                             | Максимальное количество символов в инпуте                        |
| ``clearable``      |     Boolean      |    ``true``    | ``true``, ``false``                                                                     | Стирать введенное значение с помощью кнопки                      |
| ``postfix``        |      String      |      ````      | Любая строка                                                                            |                                                                  |

2. объект `styleConfig`:
Набор стилей для кастомизация Input.

### События
| Название              | Тип параметров | Описание                                          |
|-----------------------|:--------------:|---------------------------------------------------|
| ``error``             |     String     | Возвращает ошибку из валидаторов                  |
| ``update:modelValue`` |     String     | Возвращает введенное значение                     |
| ``keydown``           |     Event      | Вызывается при нажатии стрелки вниз на клавиатуре |
| `labelDisabledColor`  |     String     | Цвет лэйбла в состоянии `disabled`                |
| `valueDisabledColor`  |     String     | Цвет значения инпута в состоянии `disabled`       |
| `helperDisabledColor` |     String     | Цвет хэлпера в состоянии `disabled`               |

#### Пример styleConfig
````javascript
styleConfig = {
        fontFamily: 'Open Sans',
        valueColor: 'black',
        valueFontWeight: '500',
        placeholderColor: 'gray',
        placeholderDisabledColor: 'gray',
        labelColor: 'black',
        labelFontWeight: '600',
        helperTextColor: 'gray',
        helperTextFontWeight: '400',
        helperTextFontSize: '10px',
        borderColor: 'gray',
        borderRadius: '6px',
        backgroundColor: 'white',
        backgroundDisabledColor: 'gray',
        focusBorderColor: 'blue',
        filledBackgroundColor: 'yellow',
        filledFontColor: 'blue',
        searchBackgroundColor: 'gray',
        iconColor: 'gray',
        errorColor: 'red',
        successColor: 'green'
      }
````

#### Пример функции валидатора

````javascript
// Валидаия на обязательное поле
const required = (value) => {
  if (
    !value ||
    value.length === 0 ||
    (value instanceof Object && !Array.isArray(value) && !value.value)
  ) {
    return 'This field is required' // Если нет значения выводим ошибку
  }
  return '' // Иначе ошибки нет
}
````