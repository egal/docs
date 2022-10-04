## Burger

Определяется тэгом:

```vue
<e-burger></e-burger>
```

Принимает 2 параметра в качестве пропа:

1. объект data, содержимое описано в таблице ниже

### Параметры объекта data

| Параметр     |   Тип   | По умолчанию | Возможные значения                       | Описание                                                                                                                                                                                                                |
|--------------|:-------:|--------------|:-----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `open`       | Boolean | `false`      | `true`, `false`                          | Определяет закрытие/открытие бургер меню                                                                                                                                                                                |
| `element`    | String  | `''`         | Любая строка с названием класса элемента | Если необходимо чтобы бургер меню разворачивалось ПОД каким-либо элементом (навбаром в хедере), нужно передать название класса этого элемента. По дефолту меню раскрываеются на всю высоту страницы и перекрывает хедер |
| `isVertical` | Boolean | `true`       | `true`, `false`                          | Определяет направление раскрытия меню в бургере. Если `isVertcal: true` - меня раскрывается сверху вниз, если `false` - справа на лево                                                                                  |

2. объект styleConfig: Набор стилей для кастомизации табов.

### Пример styleConfig (в примере указаны дефолтные значения)

```javascript
styleConfig = {
    fontFamily: 'Inter',        // ШрифтoverlayBackground: '#fff',   // Wdtn ajyf hfcrhsnjuj vty.
    dashColor: '#2d3748',        // Цвет горизонтальных линий
    backgroundHover: '#e2e8f0',  // Цвет кнопки с бургером при наведении
    background: '#edf2f7',       // Цвет кнопки с бургером
}
```

### Пример использования

```vue
<template>
  <div class="header">
    <div class="logo">LOGO</div>
    <div class="links">link1 link2 link3</div>
    <!--  Пример закрытия/открытия бургера по клику на кнопку,  -->
    <!--  может использоваться для закрытия бургера по клику на ссылку в навбаре и т.п. -->
    <button @click="isopen = !isopen">toggle burger from outside</button>
    
    <EBurger :data="{ element: 'header', isVertical: false, open: isopen }">
      <template #default> menu burger content </template>
    </EBurger>
    
  </div>
  <div>
    here is some main content
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import EBurger from '@/components/navigation/EBurger.vue'
export default defineComponent({
  name: 'App',
  components: { EBurger },
  data() {
    return {
      isopen: false,
    }
  },
  methods: {},
})
</script>

<style lang="scss">
* {
  margin: 0;
}

.header {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  border-bottom: 1px solid;
  z-index: 110;
}
</style>
```

