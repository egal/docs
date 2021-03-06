## Пагинация

Определяется тэгом:
```vue
<e-pagination></e-pagination>
```

Компонент пагинации принимает следующие параметры в качестве пропов:

1) Объект `data` который может содержать следующие параметры:
если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр                   | Что делает                                                                                                                      | Доступные значения              | По-умолчанию  |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|---------------------------------|---------------|
| ``numberOfPages``          | Общее количество страниц                                                                                                        | number                          | ``0``         |
| ``size?``                  | Размер                                                                                                                          | "sm", "md", "lg"                | ``md``        |
| ``font?``                  | Шрифт                                                                                                                           | "Inter", "Open Sans", "Raleway" | ``Open Sans`` |
| ``weight?``                | Толщина шрифта                                                                                                                  | "medium", "regular", "bold"     | ``regular``   |
| ``color?``                 | Цвет шрифта (обычный)                                                                                                           | string                          | `` ``         |
| ``activeColor?``           | Цвет шрифта (активный)                                                                                                          | string                          | `` ``         |
| ``activeBackgroundColor?`` | Цвет заднего плана (активный)                                                                                                   | string                          | `` ``         |
| ``borderColor?``           | Цвет рамки                                                                                                                      | string                          | `` ``         |
| ``perPage``                | Количество записей на страницу                                                                                                  | number                          | ``10``        |
| ``dropdownPosition``       | Установка позиционирования списка опций                                                                                         | ``bottom``, ``top``             | ``bottom``    |                                 |
| ``selectOptions``          | Массив их объектов типа `{ name: 5 }`. Отвечает за опции, которые можно выбрать в дропдауне (количество элементов на страницу). | object;                         | 5, 10, 25, 50 |
| ``hoverBackgroundColor``   | Цвет фона у номера страницы при наведении                                                                                       | string;                         | `'#A6C8FA'`   |
| ``pressedBackgroundColor`` | Цвет фона у номера страницы при нажатии                                                                                         | string;                         | `'#76ACFB'`   |
| ``isPerPageSelect``        | Определяет отображение селекта с выбором количества вывода элементов на страницу                                                | boolean;                        | `false`       |
| ``variant``                | Стиль компонента                                                                                                                | `primary`, `clear`              | `primary`     |
| ``perPageLabel``           | Текст около селекта с выбором количества элементов на страницу                                                                  | string;                         | `'Show: '`    |

2) `minimalisticVersion` - проп для определения "типа" пагинации:

| Параметр              | Что делает                                                                                                                                                                  | Доступные значения | По-умолчанию |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|--------------|
| `minimalisticVersion` | Обычное  переключение страниц (`minimalisticVersion: false`) или пагинация с селектом для выбора количества отображаемых элементов на странице(`minimalisticVersion: true`) | boolean            | `false`      |

3) `selectStyleConfig` - проп для задания пользовательских стилей селекту
4) `dropdownStyleConfig` - проп для задания пользовательских стилей дропдауну селекта

### События
| Название               | Тип параметров | Описание                                                                   |
|------------------------|----------------|----------------------------------------------------------------------------|
| `@update:perPageValue` | Object         | Эмит при изменении в селекте количества отображаемых элементов на страницу |
