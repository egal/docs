## Тэг

Определяется тэгом:
```vue
<e-tag></e-tag>
```

Компонент тэга принимает 2 параметра в качестве пропов:

### Объект `data` 
Может содержать следующие параметры:
если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр         | Что делает                                                                      | Доступные значения            | По-умолчанию |
|------------------|---------------------------------------------------------------------------------|-------------------------------|--------------|
| `size`           | Определяет размер компонента                                                    | `sm`, `md`, `lg`              | `md`         |
| `shape`          | Определяет форму компонента                                                     | `square`, `circle`            | `square`     |
| `icon`           | определяет, какая будет иконка слева (если значение пустое, то иконки не будет) | `string`                      | ``           |
| `crossIcon`      | Определяет, будет ли иконка закрытия                                            | `boolean`                     | `false`      |
| `componentStyle` | Определяет цветовой стиль самого компонента                                     | `light`, `normal`, `outlined` | `normal`     |
| `textStyle`      | Определяет цветовой стиль текста компонента                                     | `light`, `normal`             | `normal`     |
| `disabled`       | Определяет заблокирован ли компонент                                            | `true`, `false`               | `false`      |


### Объект `styleConfig`: Набор стилей для кастомизации

| Параметр  | Тип    | По умолчанию | Описание                                       |
|:----------|:-------|:-------------|:-----------------------------------------------|
| `font`    | String | `Open Sans`  | Определяет шрифт, используемый в компоненте    |
| `weight`  | String | `500`        | Определяет font-weight для шрифта в компоненте |


### События
| Название        | Описание                                           |
|:----------------|:---------------------------------------------------|
| `on:close`      | Эмитит событие клика на иконку крестика (закрытия) |
| `on:icon-click` | Эмитит событие клика на пользовательскую иконку    |