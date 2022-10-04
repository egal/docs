## ESnackbar

Определяется тэгом:

```vue
<e-snackbar></e-snackbar>
```

Принимает 2 параметра в качестве пропа:

1. объект data, содержимое описано в таблице ниже

### Параметры объекта data

| Параметр    |   Тип   | По умолчанию                                     | Возможные значения                                                                  | Описание                                                                                                                                                                                                                     |
|-------------|:-------:|--------------------------------------------------|:------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `position`  | Object  | `{ left: '', right: '0', top: '', bottom: '0' }` | `'0', '20px', '50%'`                                                                | Определяет расположение снакбара на экране. Передавать можно то же, что и для CSS параметров `left`, `right`, `top` и `bottom`, но строкой                                                                                   |
| `header`    | String  | `''`                                             | Любая строка или пустая строка                                                      | Заголовок снакбара. Заголовка не будет если передавать пустую строку                                                                                                                                                         |
| `text`      | String  | `''`                                             | Любая строка                                                                        | Основной текст внутри снакбара                                                                                                                                                                                               |
| `icon`      | String  | `''`                                             | Любое имя иконки, которое есть в Bootstrap (`circle-fill`, `chevron-right`, и т.д.) | Иконка справа. Если значение пустое, то иконка не отображается                                                                                                                                                               |
| `closeIcon` | Boolean | `false`                                          | `true`, `false`                                                                     | Определяет отображение икноки с керстиком для закрытия. По клику на иконку эмитится событие `close`                                                                                                                          |
| `type`      | String  | `''`                                             | `'clear'`, `'warning'`, `'danger'`, `'info'`, `'success'`, `''`                     | Определяет цвет границы справа. Для задания кастомного цвета границы - нужно передавать цвет в `styleConfig.borderColor`. Тип `clear` означает отстувие границы                                                              |
| `open`      | Boolean | `false`                                          | `true`, `false`                                                                     | Открытие\закрытие снакбара                                                                                                                                                                                                   |
| `delay`     | Number  | `0`                                              | Любое число (`3000`, `5000`)                                                        | Время отображения снакбара (**в миллисекундах**). После истечения времени - эмитит событие `close`, в котором также передается флаг `timer`, если понадобится различать закрытие по крестику и закрытие по истечению времени |
| `showTimer` | Boolean | `false`                                          | `true`, `false`                                                                     | Определяет отображение иконки таймера                                                                                                                                                                                        |

2. объект styleConfig: Набор стилей для кастомизации табов.

### Пример styleConfig (в примере указаны дефолтные значения)

```javascript
styleConfig = {
    padding: '16px', // padding контейнера
    fontFamily: 'Inter', 
    fontSize: '14px',
    headerColor: '#A0AEC0', // Цвет заголовка
    textColor: '#A0AEC0',      // Цвет текста в контекйнере
    borderColor: '#2D3748',   // Цвет границы
    iconColor: '#718096',  // Цвет иконки справа или иконки таймера
    width: '325px',  // Ширина контейнера
}
```
3. Чтобы отобразить что-либо в снакбаре после основного содержимого (например, кнопки) - есть слот с именем #footer. По дефолту пустой

### Пример использования

```vue
<template>
  <div>
    <EButton @click="isSnackbarOpen = !isSnackbarOpen">Show Snackbar</EButton>
    <ESnackbar
      :data="{
        open: isSnackbarOpen,
        header: 'Заголовок',
        text: 'Нейтральное сообщение о событии, которое не несет статусного смысла',
        icon: 'exclamation-circle',
        closeIcon: true,
        type: 'clear',
        delay: 5000,
      }"
      @close="onClose"
    >
      <template #footer>
        <div class="buttons">
          <EButton :data="{ light: true }">Button 1</EButton>
          <EButton :data="{ light: true }">Button 2</EButton>
        </div>
      </template>
    </ESnackbar>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import ESnackbar from '@/components/snackbar/ESnackbar.vue'
import EButton from '@/components/togglers/EButton.vue'
export default defineComponent({
  name: 'App',
  components: { ESnackbar, EButton },
  data() {
    return {
      isSnackbarOpen: false,
    }
  },
  methods: {
    onClose(isClosedByTimer: string) {
      // do something on close
      
      if (isClosedByTimer) {
        console.log('closed by timer')
      } else {
        console.log('closed by icon click')
      }
       
      this.isSnackbarOpen = false
    },
  },
})
</script>
```

### События

| Название | Тип параметров | Описание                                                                                                                                      |
|----------|:--------------:|-----------------------------------------------------------------------------------------------------------------------------------------------|
| `close`  |     String     | Срабатывает при клике на иконку закрытия или по истечению таймера. В последнем случае - в параметрах дополнительно передается флаг `'timer'`. |

