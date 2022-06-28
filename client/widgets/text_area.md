## Textarea

Определяется тэгом:
```vue
<e-textarea></e-textarea>
```

### Параметры
Textarea принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр       |   Тип   | По умолчанию | Возможные значения             | Описание                                                         |
|----------------|:-------:|:------------:|--------------------------------|------------------------------------------------------------------|
| ``paceholder`` | String  |    ``''``    | Любая строка                   | Placeholder                                                      |
| ``label``      | String  |    ``''``    | Любая строка                   | Label                                                            |
| ``helperText`` | String  |    ``''``    | Любая строка                   | Мелкий текст под полем ввода                                     |
| ``error``      | String  |    ``''``    | Любая строка                   | Вывод кастомной ошибки                                           |
| ``modelValue`` | String  |    ``''``    | Любая строка                   | Значение инпута (можно использовать v-model)                     |
| ``validators`` |  Array  |    ``[]``    | Массив функций-валидаторов     | Проверка значения будет проходить через каждую функцию-валидатор |
| ``size``       | String  |    ``md``    | ``sm``, ``md``, ``lg``, ``xs`` | Размер поля ввода                                                |
| ``disabled``   | Boolean |  ``false``   | ``true``, ``false``            | Заблокировать поле ввода                                         |
| `showSuccess`  | Boolean |  ``false``   | ``true``, ``false``            | Показывать "успешное" состояние поля ввода                       |

2. объект `styleConfig`:
Набор стилей для кастомизация Textarea.

| Параметр                   | Тип    | По умолчанию | Возможные значения | Описание                                        |
|:---------------------------|:-------|:-------------|:-------------------|:------------------------------------------------|
| `valueDisabledColor`       | String | `#CBD5E0`    |                    | Цвет значения поля ввода в состоянии `disabled` |
| `placeholderDisabledColor` | String | `#CBD5E0`    |                    | Цвет плейсхолдера в состоянии `disabled`        |
| `backgroundDisabledColor`  | String | `#ffffff`    |                    | Цвет background в состоянии `disabled`          |
| `labelDisabledColor`       | String | `#CBD5E0`    |                    | Цвет лейбла в состоянии `disabled`              |
| `helperTextDisabledColor`  | String | `#CBD5E0`    |                    | Цвет хэлпера в состоянии `disabled`             |
| `helperTextFontSize`       | String | `12px`       |                    | Размер шрифта хэлпера                           |
| `successСolor`             | String | `#66CB9F`    |                    | Показывать "успешное" состояние поля ввода      |

### События
| Название              | Тип параметров | Описание                             |
|-----------------------|:--------------:|--------------------------------------|
| ``error``             |     String     | Возвращает ошибку из валидаторов     |
| ``update:modelValue`` |     String     | Возвращает выбранное(ые) значение(я) |

#### Пример styleConfig
````javascript
styleConfig = {
        fontFamily: 'Inter',
        valueColor: 'black',
        valueFontWeight: '500',
        placeholderColor: 'gray',
        labelColor: 'black',
        labelFontWeight: '600',
        helperTextColor: 'gray',
        helperTextFontWeight: '400',
        borderColor: 'gray',
        borderRadius: '6px',
        backgroundColor: 'white',
        focusBorderColor: 'blue',
        errorColor: 'red',
      }
````