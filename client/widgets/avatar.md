## Аватар

Определяется тэгом:
```vue
<e-avatar></e-avatar>
```

Компонент аватара принимает 1 параметр в качестве пропа:

Объект `data` который может содержать следующие параметры:
если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр          | Что делает                                                                                                                        | Доступные значения                     | По-умолчанию  |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------|----------------------------------------|---------------|
| `imgUrl`          | Принимает ссылку на аватар                                                                                                        | `string`                               |               |
| `name`            | Имя. На аватаре будут отображаться только первые буквы первого и последнего слова в имени                                         | `string`                               |
| `size`            | Размер                                                                                                                            | `'xs'`, `'sm'`, `'md'`, `'lg'`, `'xl'` | `md`          |
| `shape`           | Форма                                                                                                                             | `square`, `circle`                     | `circle`      |
| `font`            | Шрифт                                                                                                                             | `Inter`, `Open Sans`, `Raleway`        | `Open Sans`   |
| `weight`          | Толщина шрифта                                                                                                                    | `medium`, `regular`, `bold`            | `bold`        |
| `color`           | Цвет текста, при пустом значении - белый                                                                                          | string                                 | ""            |
| `bgColor`         | Цвет заднего фона, отображается если нет изображения, по-умолчанию серый                                                          | string                                 | ""            |
| `darken`          | Затемнить картинку                                                                                                                | boolean                                | false         |
| `borderColor`     | Цвет маленькой рамки вокруг online-индикатора (по-умолчанию белый)                                                                | string                                 | ""            |
| `isOnline`        | Отображать ли online-индикатор                                                                                                    | boolean                                | false         |
| `isNameRequired`  | Отображать ли имя даже в случае наличия картинки                                                                                  | boolean                                | false         |
| `isDropdownOpen`  | Определяет открытие/хакрытие дродауна                                                                                             | boolean                                | false         |
| `dropdownOptions` | Содержимое дродауна. принимает массив объектов вида `{name: 'First option, leftIcon: 'iconName', rightIcon: 'iconName'}`          | array                                  | []            |
| `editable`        | Определяет можно ли загружать новое изображение                                                                                   | boolean                                | false         |
| `isLoading`       | Определяет показывать ли текст `'Loading...'` (по умолчанию) во время загрузки изображения                                        | boolean                                | false         |
| `loadingText`     | Текст во время загрузки изображения                                                                                               | string                                 | `'isLoading'` |
| `multipleAvatars` | Массив с данными для отображения группы аваторок. Может содержать список объектов такими уникальными параметры как имя, цвет фона | array                                  | []            |
| `borderColor`     | Цвет рамки каждой аватраки в группе автарок                                                                                       | string                                 | `#ffffff`     |

## Пример использования
````vue
 <template>
  <div>
    <!--  Пример вызова компонента с дропдауном -->
    <EAvatar
        :data="{
        name: 'John Doe1',
        dropdownOptions: [{ name: 'First option' }, { name: 'Second option' }],
      }"
        @select="handleSelect"
    />
    <br />
    <!--  Пример вызова компонента с возможностью загрузки нового изображения -->
    <EAvatar
        :data="{ name: 'John Doe2', editable: true, isLoading: load }"
        @upload="uploadImage"
        @error="logError"
    />
    <br />
    <!--  Пример вызова компонента с группой автарок -->
    <EAvatar
        :data="{
        size: 'lg',
        multipleAvatars: [
          { name: 'One One' },
          { name: 'Two Two', bgColor: 'pink' },
          {
            name: 'Three Three',
            imgUrl:
              'изображение в формате base64',
          },
        ],
      }"
    />
    <br />
    <button @click="load = !load">Toggle Loading status</button>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import EAvatar from '@/components/personality/EAvatar.vue'
import EAvatarGroup from '@/components/personality/avatar/EAvatarGroup.vue'
export default defineComponent({
  name: 'App',
  components: { EAvatarGroup, EAvatar },
  data() {
    return {
      load: false,
    }
  },
  methods: {
    handleSelect(option) {
      // option = {name: 'First option'}
    },
    uploadImage(file) {
      // file = new File object
      
      // additional checks
      // format to base64 or any other type
      // send POST request
      console.log('success upload')
    },
    logError(error) {
      // error = 'Only .png and .jpg files can be uploaded to the field'
    },
  },
})
</script>
<style lang="scss">
</style>
````