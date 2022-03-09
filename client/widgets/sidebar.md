## Навигация

Для навигации можно использовать 2 компонента на выбор: вертикальное меню слева и горизонтальное меню в заголовке страницы.
Оба компонента принимают 1 параметр в качестве пропа:

Объект `data` который может содержать следующие параметры:
если какой-то из параметров не передан, будет применено дефолтное значение.

### Sidebar
Определяется тэгом:
```vue
<e-navbar-left></e-navbar-left>
```

| Параметр      | Что делает                                              | Доступные значения              | По-умолчанию |
|---------------|---------------------------------------------------------|---------------------------------|--------------|
| logo          | Большой логотип, отбражается в открытом состоянии       | string                          | required     |
| smallLogo     | Маленький логотип, отбражается в закрытом состоянии     | string                          | required     |
| links         | Массив со объектами ссылок, `[{to: '/', name: 'Home'}]` | Link[]                          | required     |
| font?         | Шрифт                                                   | "Inter", "Open Sans", "Raleway" | "Open Sans"  |
| weight?       | Толщина шрифта                                          | "medium", "regular", "bold"     | "bold"       |
| color?        | Цвет неактивного текста                                 | string                          | ""           |
| activeColor?  | Цвет активного текста или при наведении                 | string                          | ""           |
| chevronColor? | Цвет стрелки                                            | string                          | ""           |

#### Slots:
Есть возможность добавить пользовательский контент в `footer` навигации:
Имя слота: `footer`

### Горизонтальное меню
Определяется тэгом:
```vue
<e-navbar-top></e-navbar-top>
```

| Параметр     | Что делает                                              | Доступные значения              | По-умолчанию |
|--------------|---------------------------------------------------------|---------------------------------|--------------|
| logo         | Большой логотип, отбражается в открытом состоянии       | string                          | required     |
| links        | Массив со объектами ссылок, `[{to: '/', name: 'Home'}]` | Link[]                          | required     |
| font?        | Шрифт                                                   | "Inter", "Open Sans", "Raleway" | "Open Sans"  |
| weight?      | Толщина шрифта                                          | "medium", "regular", "bold"     | "bold"       |
| color?       | Цвет неактивного текста                                 | string                          | ""           |
| activeColor? | Цвет активного текста или при наведении                 | string                          | ""           |

Имеет именованный слот avatar для аватара, не имеет параметров