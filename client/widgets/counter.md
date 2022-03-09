## Counter

Определяется тэгом:
```vue
<e-counter></e-counter>
```

### Параметры
Counter принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр       |  Тип   | По умолчанию | Возможные значения     | Описание                                       |
|----------------|:------:|:------------:|------------------------|------------------------------------------------|
| ``label``      | String |    ``''``    | Любая строка           | Label                                          |
| ``helperText`` | String |    ``''``    | Любая строка           | Мелкий текст под счетчиком                     |
| ``modelValue`` | Number |    ``0``     | Любое число            | Значение счетчика (можно использовать v-model) |
| ``min``        | Number |   ``null``   | Любое число            | Минимальное возможное значение                 |
| ``max``        | Number |   ``null``   | Любое число            | Максимальное возможное значение                |
| ``size``       | String |    ``md``    | ``sm``, ``md``, ``lg`` | Размер счетчика                                |

2. объект `styleConfig`:
Набор стилей для кастомизация счетчика.

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