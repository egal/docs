## Counter

Определяется тэгом:
```vue
<e-counter></e-counter>
```

### Параметры
Counter принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр       |   Тип   | По умолчанию | Возможные значения           | Описание                                       |
|----------------|:-------:|:------------:|------------------------------|------------------------------------------------|
| ``label``      | String  |    ``''``    | Любая строка                 | Label                                          |
| ``helperText`` | String  |    ``''``    | Любая строка                 | Мелкий текст под счетчиком                     |
| ``modelValue`` | Number  |    ``0``     | Любое число                  | Значение счетчика (можно использовать v-model) |
| ``min``        | Number  |   ``null``   | Любое число                  | Минимальное возможное значение                 |
| ``max``        | Number  |   ``null``   | Любое число                  | Максимальное возможное значение                |
| ``size``       | String  |    ``md``    | `xs`, ``sm``, ``md``, ``lg`` | Размер счетчика                                |
| `disabled`     | Boolean |    false     | true, false                  | Заблокировать счетчик                          |
| `type`         | String  |  `primary`   | `primary`, `secondary`       | Изменить внешний вид счетчика                  |

2. объект `styleConfig`:
Набор стилей для кастомизация счетчика.

| Параметр                  |  Тип   | По умолчанию | Возможные значения | Описание                                        |
|---------------------------|:------:|:------------:|--------------------|-------------------------------------------------|
| `valueFontSize`           | String |    `16px`    |                    | Цвет значения поля ввода в состоянии `disabled` |
| `valueColorDisabled`      | String |  `#CBD5E0`   |                    | Цвет плейсхолдера в состоянии `disabled`        |
| `labelColorDisabled`      | String |  `#CBD5E0`   |                    | Цвет background в состоянии `disabled`          |
| `helperTextColorDisabled` | String |  `#CBD5E0`   |                    | Цвет лейбла в состоянии `disabled`              |
| `borderFocusColor`        | String |  `#76ACFB`   |                    | Цвет хэлпера в состоянии `disabled`             |
| `iconColorHover`          | String |  `#2D3748`   |                    | Размер шрифта хэлпера                           |
| `iconColorDisabled`       | String |  `#EDF2F7`   |                    | Показывать "успешное" состояние поля ввода      |

### События
| Название              | Тип параметров | Описание                      |
|-----------------------|:--------------:|-------------------------------|
| ``update:modelValue`` |     String     | Возвращает выбранное значение |

#### Пример styleConfig
````javascript
styleConfig = {
        fontFamily: 'Open Sans',
        valueColor: 'black',
        valueFontWeight: '500',
        labelColor: 'black',
        labelFontWeight: '600',
        helperTextColor: 'gray',
        helperTextFontWeight: '400',
        borderColor: 'gray',
        borderRadius: '6px',
        iconColor: 'gray'
      }
````
