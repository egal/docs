## Dot Menu
Определяется тегом:
````javascript
<e-dot-menu></e-dot-menu>
````

### Параметры
Компонент Dot Menu принимает 2 параметра в качество пропов:
1. Объект `data`, который может содержать следующие параметры:

| Параметр     |   Тип   | По умолчанию | Возможные значения                           | Описание                                                                                                       |
|--------------|:-------:|:------------:|----------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| ``items``    |  Array  |    ``[]``    | Массив объектов для отрисовки элементов меню | Описание передаваемых объектов представлено ниже                                                               |
| ``vertical`` | Boolean |   ``true``   | ``true``, ``false``                          | Определяет положение иконки с тремя точками (горизонтальное/вертикальное)                                      |
| ``disabled`` | Boolean |  ``false``   | ``true``, ``false``                          | Состояние меню (заблокировано/разблокировано)                                                                  |
| ``isOpen``   | Boolean |  ``false``   | ``true``, ``false``                          | Определяет, открыто ли меню изначально. Позволяет влиять на открытие/закрытие меню из родительского компонента |
| ``size``     | String  |   ``'md'``   | ``'sm'``, ``'md'``, ``'lg'``, ``xs``         | Размер меню                                                                                                    |
| `position`   | String  |  `'right'`   | `'left'`, `'right'`                          | Расположение выпадающего меню (внизу слева или внизу справа относительно кнопки открытия меню)                 |

## Передача параметров для отрисовки списка элементов в выпадающем меню
Содержимое выпадающего меню определяется объектами, передаваемыми в свойстве `items`
Объект может содержать следющие свойства:

| Параметр           |   Тип    | Обязательный | Возможные значения                     | Описание                                                                      |
|--------------------|:--------:|:------------:|----------------------------------------|-------------------------------------------------------------------------------|
| ``label``          |  String  |      да      | Любая строка                           | Текст элемента списка                                                         |
| ``icon``           |  String  |     нет      | Строка с названием иконки из Bootstrap | Иконка элемента                                                               |
| ``onClickHandler`` | Function |     нет      | Функция                                | Функция, определяющая что должно проийзоти после клика на этот элемент списка |
| `iconRight`        |  String  |     нет      | Строка с названием иконки из Bootstrap | Иконка элемента справа от текста                                              |

## Divider
Чтобы отрисовать разделитель между элементами в списке меню, нужно передать в массив `items` объект вида:
````javascript
{
    label: 'divider'
}
````

2. Объект `styleConfig`: Набор стилей для кастомизации

| Параметр                    |  По умолчанию  | Описание                                                                                                   |
|-----------------------------|:--------------:|------------------------------------------------------------------------------------------------------------|
| `fontFamily`	               |  `Open Sans`	  | Шрифт текста в выпадаеющем меню                                                                            |
| `fontWeight`	               |     `500`      | 	Свойство font-weight текста в выпадаеющем меню                                                            |
| `textColor`	                |  `'#2D3748'`   | Цвет текста в выпадающем меню                                                                              | 
| `backgroundHover`           | 	`''#E2E8F0''` | 	Цвет фона при наведении на один из элементов списка меню                                                  |
| `backgroundPressed`         |  `'#cbd5e0'`   | Цвет фона при нажатии на элемент в выпадающем меню                                                         |
| `backgroundColor`           |   `'#fff'`	    | Цвет фона в выпадающем меню и у кнопки с 3мя точками                                                       |
| `iconColor`	                |  `'#2D3748'`   | 	Цвет иконки с 3мя точками                                                                                 |
| `disabled`	                 |       -        | 	Объект, содержащий стили для заблокированного состояния. Содержит 2 свойства: iconColor и backgroundColor |
| `disabled.iconColor`	       |  `'#CBD5E0 '`  | 	Цвет иконки с 3мя точками в заблокированном состоянии                                                     |
| `disabled.backgroundColor`	 |    `'#fff'`    | 	Цвет фона у кнопки с 3мя точками в заблокированном состоянии                                              |

## Пример использования
````vue
<template>
  <EDotMenu
      :data="{
       items: [
          {
            label: 'Archive',
            icon: 'archive',
            iconRight: 'archive',
            onClickHandler: () => {
              console.log('example')
            },
          },
          {
            label: 'divider',
          },
          { label: 'Trash', icon: 'trash', isDisabled: true },
        ],
      vertical: false,
      disabled: false,
      isOpen: false,
      size: 'lg',
    }"
      :style-config="{
          fontFamily: 'Raleway',
          iconColor: '#000000',
          disabled: {
            iconColor: '#4A5568',
            backgroundColor: 'grey'
          },
      }"
  />
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import EDotMenu from '@/components/navigation/EDotMenu.vue'
export default defineComponent({
  name: 'App',
  components: { EDotMenu },
  data() {
    return {
    }
  },
  methods: {
    openDeleteModal() {
      // opening Modal Window
    },
    setEditMode() {
      // setting Edit Mode...
    },
  },
})
</script>
````

### События

| Название              | Тип параметров | Описание                                                                                                     |
|-----------------------|:--------------:|--------------------------------------------------------------------------------------------------------------|
| ``open``              |     Event      | Эмитит событие открытия выпадающего меню (если необходимо отследить это событие в родительском компоненте)   |
| ``close``             |     Event      | Эмитит событие закрытия выпадающего меню (если необходимо отследить это событие в родительском компоненте)   |