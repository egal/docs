## Навигация

Для навигации можно использовать 2 компонента на выбор: вертикальное меню слева и горизонтальное меню в заголовке страницы.

### Sidebar
Определяется тэгом:
```vue
<e-navbar-left></e-navbar-left>
```
Принимает 2 параметра в качестве пропа: ``data`` и ``style-config``.
1) Объект `data`, который может содержать следующие параметры (если какой-то из параметров не передан, будет применено дефолтное значение):

| Параметр         | Что делает                                              | Доступные значения | По-умолчанию    |
|------------------|---------------------------------------------------------|--------------------|-----------------|
| ``logo``         | Большой логотип, отбражается в открытом состоянии       | string             | ""              |
| ``smallLogo``    | Маленький логотип, отбражается в закрытом состоянии     | string             | ""              |
| ``links``        | Массив со объектами ссылок, `[{to: '/', name: 'Home'}]` | string             | ""              |
| ``verticalDash`` | Цвет стрелки                                            | true, false        | true            |
| `filled`         | Boolean                                                 | `true`             | `true`, `false` | Определяет стиль элементов в списке сайдбара. Если `true` - у элемента будет появляться бэкграунд при навдении/клике |

2) Объект `styleConfig`, который может содержать следующие параметры (если какой-то из параметров не передан, будет применено дефолтное значение):

| Параметр                   |  Тип   | По умолчанию  | Возможные значения | Описание                                                     |
|----------------------------|:------:|:-------------:|--------------------|--------------------------------------------------------------|
| ``fontFamily``             | String | ``Open Sans`` | Любая строка       | font-family                                                  |
| ``fontWeight``             | String |    ``700``    | Любая строка       | font-weight                                                  |
| ``chevronColor``           | String |  ``#a0aec0``  | Любая строка       | Цвет стрелочки у раскрывающегося элемента списка             |
| ``active``                 | Object |    ``{}``     |                    | Объект со стилями для текущей активной ссылки                |
| ``active.textColor``       | String |    ``''``     | Любая строка       | Цвет текста и иконки у активной ссылки                       |
| ``active.backgroundColor`` | String |    ``''``     | Любая строка       | Цвет фона активной ссылки                                    |
| ``hover``                  | Object |    ``{}``     |                    | Объект со стилями для ссылки при наведении                   |
| ``hover.textColor``        | String |    ``''``     | Любая строка       | Цвет текста и иконки при наведении                           |
| ``hover.backgroundColor``  | String |    ``''``     | `   Любая строка   | Цвет фона при наведении                                      |
| ``listItemPadding``        | String | ``'10px 0'``  | Любая строка       | Паддинг для элемента списка в сайдбаре                       |
| ``ulGap``                  | String |  ``'12px'``   | Любая строка       | Дополнительное расстояние между элементами в списке сайдбара |
| ``borderRadius``           | String |     ``0``     | Любая строка       | border-radius элемента списка в сайдбаре                     |
| ``listItemPadding``        | String | ``'0 20px'``  | Любая строка       | Паддинг одного элемента в списке                             |
| ``listItemHeight``         | String |   ``'40'``    | Любая строка       | Высота элемента в списке сайдбара                            |


Пример объекта `styleConfig`:
````javascript
      styleConfig: {
        fontFamily: 'Raleway',
        fontWeight: '700',
        chevronColor: 'red',
        textColor: '#262525',
        backgroundColor: 'transparent',
        borderRadius: '8px',
        listItemPadding: '12px 20px',
        ulGap: '5px',
        verticalDash: true,
        listItemPadding: '0 20px',
        listItemHeight: '40',     // Высота элемента в списке сайдбара
        
        active: {
          textColor: '#fff',
          backgroundColor: '#5377bb',
        },
        hover: {
          textColor: '#215ab6',
          backgroundColor: '#b8cfff',
        },
      },
````

#### Slots:
1) Есть возможность добавить пользовательский контент в `footer` навигации:
Имя слота: `footer`
2) Есть возможность добавить пользовательский контент к каждой ссылке (будет отображаться справа от нее):
```vue
 <ENavbarLeft :data="sidebarConfig" :style-config="styleConfig">
    <template v-slot:badge="{ linkName }">
      <span v-if="linkName === 'Главная'">icon</span>
    </template>
  </ENavbarLeft>
```
### Горизонтальное меню
Определяется тэгом:
```vue
<e-navbar-top></e-navbar-top>
```
Принимает 1 параметр в качестве пропа.
Объект `data`, который может содержать следующие параметры (если какой-то из параметров не передан, будет применено дефолтное значение):
| Параметр     | Что делает                                              | Доступные значения              | По-умолчанию |
|--------------|---------------------------------------------------------|---------------------------------|--------------|
| logo         | Большой логотип, отбражается в открытом состоянии       | string                          | required     |
| links        | Массив со объектами ссылок, `[{to: '/', name: 'Home'}]` | Link[]                          | required     |
| font?        | Шрифт                                                   | "Inter", "Open Sans", "Raleway" | "Open Sans"  |
| weight?      | Толщина шрифта                                          | "medium", "regular", "bold"     | "bold"       |
| color?       | Цвет неактивного текста                                 | string                          | ""           |
| activeColor? | Цвет активного текста или при наведении                 | string                          | ""           |

Имеет именованный слот avatar для аватара, не имеет параметров

### Пример использования
````vue
<template>
  <div class="wrapper">
    <ENavbarLeft
      :data="{
        filled: true,
        verticalDash: false,
        logo: 'src/logo-sign.svg',
        links: [
          {
            to: '/1',
            name: 'Dashboard',
            icon: 'layout-text-window',
            links: [
              {
                to: '/2',
                name: '222',
                icon: 'person',
              },
              {
                to: '/3',
                name: '333',
                icon: 'person',
              },
              {
                to: '/4',
                name: '444',
                icon: 'person',
              },
            ],
          },
          {
            to: '/5',
            name: 'User',
            icon: 'person',
          },
          {
            to: '/6',
            name: 'Saved',
            icon: 'heart',
          },
        ],
      }"
    >
      <template #content>
        <div class="content">
         Some long scrollable content   
        </div>
      </template>
    </ENavbarLeft>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import ENavbarLeft from '@/components/navigation/ENavbarLeft.vue'

export default defineComponent({
  name: 'App',
  components: { ENavbarLeft },
  data() {
    return {}
  },
  methods: {},
})
</script>

<style lang="scss">
.content {
  padding: 20px;
}
</style>

````

### Примечания
Можно передавать содержимое страницы в слот с именем #content, чтобы сайдбар "закреплялся слева и не прокручивался при скролле страницы.
