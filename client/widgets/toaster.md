# Toaster

Плагин уведомлений.

## Подключение плагина
`App.vue`:
````vue
<template>
  <div>
    <ToasterContainer />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ToasterContainer from '@egalteam/widget-library';

export default defineComponent({
  name: "App",
  components: {
    ToasterContainer,
  },
});
</script>
````

## Использование
````typescript
this.$toaster.info({ message: 'This is Info toast!' });
````
````typescript
this.$toaster.danger({ message: 'This is Danger toast!' });
````
В тостер можно передавать текст с HTML тегами через свойство `rawHtml`. Пример:
````javascript
this.$toaster.info({
      title: '',
      rawHtml: 'text must be <span style="color: red">red</span>',
      flat: true,
    })
````

### Конфигурация
Глобальные конфиги устанавливаются через входные параметры компонента `ToasterContainer`

| Параметр            |      Тип       |       По умолчанию       | Возможные значения                                             | Описание                                                                                    |
|---------------------|:--------------:|:------------------------:|----------------------------------------------------------------|:--------------------------------------------------------------------------------------------|
| `position`          |     String     |      `'top-right'`       | `'top-left'`, `'top-right'`, `'bottom-right'`, `'bottom-left'` | Расположение тостера относительно экрана                                                    |
| `reversed`          |    Boolean     |         `false`          | `true`, `false`                                                | Изменяет метод добавления тостов (`true` - добавляет в начало, `false` - добавляет в конец) |
| `duration`          | Number, String |          `3000`          | Любое число                                                    | Длительность отображения тоста                                                              |
| `globalVariant`     |     String     |        `'light'`         | `'light'`, `'dark'`                                            | Цветовая схема тостов                                                                       |
| `globalInfoTitle`   |     String     |     `'Information'`      | Любая строка                                                   | Заголовок тостов типа `info`                                                                |
| `globalInfoIcon`    |     String     |     `'info-circle'`      | Любая иконка, доступная в Bootstrap                            | Иконка тостов типа `info`                                                                   |
| `globalDangerTitle` |     String     |        `'Danger'`        | Любая строка                                                   | Заголовок тостов типа `danger`                                                              |
| `globalDangerIcon`  |     String     | `'exclamation-triangle'` | Любая иконка, доступная в Bootstrap                            | Иконка тостов типа `danger`                                                                 |
| `globalFlat`        |    Boolean     |         `false`          | `true`, `false`                                                | Вид тостов (`true` - тосты в одну строчку)                                                  |

Конфиги для тоста устанавливаются при вызове:
````typescript
this.$toaster.info({
        title: "Info toast!",
        message: "Hi there, I'm Info!",
        variant: "dark",
        flat: true
      });
````

| Параметр                  | Обязательный |   Тип    | Возможные значения                  | Описание                                 |
|---------------------------|:------------:|:--------:|-------------------------------------|:-----------------------------------------|
| `message`                 |    `true`    |  String  | Любая строка                        | Сообщение тоста                          |
| `title`                   |   `false`    |  String  | Любая строка                        | Заголовок тоста                          |
| `icon`                    |   `false`    |  String  | Любая иконка, доступная в Bootstrap | Иконка тоста                             |
| `variant`                 |   `false`    |  String  | `'light'`, `'dark'`                 | Цветовая схема тоста                     |
| `flat`                    |   `false`    | Boolean  | `true`, `false`                     | Вид тоста (`true` - тост в одну строчку) |
| `primaryAction`           |   `false`    |  String  | Любая строка                        | Текст главной кнопки тоста               |
| `primaryActionCallback`   |   `false`    | Function | Любая функция                       | Коллбэк для главной кнопки тоста         |
| `secondaryAction`         |   `false`    |  String  | Любая строка                        | Текст главной кнопки тоста               |
| `secondaryActionCallback` |   `false`    | Function | Любая функция                       | Коллбэк для главной кнопки тоста         |

`styleConfig` (тип: Object) для компонента `ToasterContainer`

### Использование styleConfig
(Только для изменения стилей заголовка (`title`), сообщения (`message`) и текста (`rawHtml`)).
Чтобы кастомизировать текст внутри тостера, нужно передать в `styleConfig` объект в виде:
````javascript
{
  title: {
    color: 'green'
  },
  message: {
   color: 'blue',
   fontSize: '18px'
  },
  rawHtml: {
    fontSize: '18px'
  },
}
````