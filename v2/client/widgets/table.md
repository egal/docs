## Table

Определяется тэгом:

```vue
<e-table></e-table>
```

Принимает 2 параметра в качестве пропа:

1. объект data, содержимое описано в таблице ниже

### Параметры объекта data

| Параметр             |     Тип     | По умолчанию | Возможные значения                                          | Описание                                                                                                                                                                                                                                                                          |
|----------------------|:-----------:|--------------|:------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `clickableRow`       |   Boolean   | `false`      | `true`, `false`                                             | Определяет вид курсора при наведении. true - курсор 'pointer', false - курсор 'default'                                                                                                                                                                                           |
| `defaultColumnWidth` |   String    | `150`        | Любое число                                                 | Ширина столбца                                                                                                                                                                                                                                                                    |
| `expandOnIcon`       |   Boolean   | `true`       | `true`, `false`                                             | Определяет как будет раскрываться строка. true - раскрывается по клику на стрелку, false - раскрывается по клику на всю строку (если одновременно задано отображение стрелки и в этом параметре передано false, строка будет раскрываться и по стрелке, и по клику на всю строку) | 
| `fixedHeader`        |   Boolean   | `false`      | `true`, `false`                                             | Определяет будет ли хедер фиксированным при скролле таблицы                                                                                                                                                                                                                       |
| `fixedHeight`        |   String    | `''`         | Любое значение высоты строкой (`'100%'`, `'100px'`, и т.д.) | Задает фиксированную высоту таблицы. Появляется вертикальный скролл, если таблица превышает фиксированную высоту                                                                                                                                                                  |
| `headerCheckbox`     |   Boolean   | `false`      | `true`, `false`                                             | Отобржать ли чекбокс в хедере (чекбокс в хедере работает по типу 'select all' и отмечает выбранными все остальные чекбоксы в таблице)                                                                                                                                             |
| `headerFields`       | HeaderTypes | `[]`         | Описание типа HeaderTypes приведено ниже                    | Определение заголовков в таблице                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                    
| `isLoading`          |   Boolean   | `false`      | `true`, `false`                                             | Отображает лоадер пока данные таблицы грузятся                                                                                                                                                                                                                                    |
| `items`              |    Array    | `[]`         | Массив с данными                                            | Данные которые будут отображаться в таблице                                                                                                                                                                                                                                       |
| `menuVisible`        |   Boolean   | `true`       | `true`, `false`                                             | Определяет отображение слота для меню с 3 точками. true - меню отображается всегда, false - только при наведении на строку)                                                                                                                                                       |
| `notFoundMessage`    |   String    | `''`         | Любая строка                                                | Текст если в таблице нет данных                                                                                                                                                                                                                                                   |
| `oneExpand`          |   Boolean   | `true`       | `true`, `false`                                             | Если true - раскрытой может оставаться только 1 строка, false - можно раскрыть несколько строк в таблице одновременно                                                                                                                                                             |
| `sort`               |   String    | `''`         | `'asc'`, `'desc'`                                           | Направление сортировки данных в столбце                                                                                                                                                                                                                                           |
| `sortField`          |   String    | `''`         | Любое `name` из `headerFields`                              | Если передана не пустая строка - определяет по какому столбцу изначально будут отсортированы данные (столбец должен отображаться в таблице), появляется иконка сортировки                                                                                                         |
| `template`           |   String    | `'default'`  | `'default'`, `'zebra'`                                      | Стиль таблицы: обычная или зебра (каждая вторая строка подсвечивается другим цветом)                                                                                                                                                                                              |
| `trackBy`            |   String    | `'id'`       | Любое `name` из `headerFields`                              | Название столбца с уникальными идентификаторами (как id). Параметр trackBy используется для сравнения отмечен ли чекбокс у строки или нет                                                                                                                                         |
| `useTransition`      |   Boolean   | `true`       | `true`, `false`                                             | Определяет будет ли использоваться анимация при разворачивании/сворачивании строки                                                                                                                                                                                                |
| `useMobileHeader`    |   Boolean   | `true`       | `true`, `false`                                             | Определяет формат отображения таблицы в мобильной версии. true - заголовки будут отображаться, false - заголовки не отображаются и значение из первого столбца выделяются жирным шрифтом                                                                                          |



2. Описание типа HeaderTypes
   Массив headerFields может содержать строки и объекты определенного вида

Строки которые можно передавать в массив:

| Строка                | Описание                                                                                     | Примечания                                                                                                                                                                                       | Что передает                                                                                                                                      |
|:----------------------|:---------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------|
| `'__slot:checkboxes'` | Для отображения слева колонки со слотами для чекбоксов                                       | Компонент чекбокса нужно передавать в слот самостоятельно. При клике на чекбоксы эмитятся соответсвующие события.                                                                                | checked - отмечен ли чекбокс в слоте (`true`, `false`). click - функция-обработчик клика на чекбокс. Обязательно нужно повесить на свой компонент |
| `'__slot:expand'`     | Для отображения слева колонки со стрелочками для разворачивания строки                       | Все строки становятся разворачиваемыми. Чтобы запретить разворачивать ОПРЕДЕЛЕННЫЕ строки, нужно дополнительно передать соответсвующему объекту в массиве `items` параметр `notExpandable: true` | item - объект из массива `data.items`. row-index - индекс текущей строки                                                                          |
| `'__slot:menu'`       | Для отображения справа слота для иконки меню (например, `EDotsMenu`)                         | Компонент меню нужно передавать в слот самостоятельно. Чтобы меню отображалось только при наведении на строку - передать в `data` `menuVisible: false`                                           | item - объект из массива `data.items`. row-index - индекс текущей строки                                                                          |
| `'__slot:actions'`    | Для отображения справа колонки со слотами для любым других элементов (кнопки, инпуты и т.д.) | Если необходимо использовать несколько слотов с типом actions, можно задать название кажому слоту, например `__slot:actions:Some action`. Название колонки будет отобрааться в хедере            | name - уникальное название слота. item - объект из массива `data.items`. row-index - индекс текущей строки                                        |

Чтобы чекбокс в слоте работал, надо передать ему параметры `checked`, полученные из слота (`checkboxProps` в примере).
Также можно получить событие  и `@click` и обработать дополнительно. При этом события клика на чекбокс эмитятся самим компонентом `ETable`
Пример использования слота с чекбоксом:
````vue
  <template #checkbox="checkboxProps">
    <input type="checkbox" :checked="checkboxProps.checked" @click="checkboxProps.click" />
  </template>
````

Интерфейс объектов которые можно передавать в массив:

| Параметр        |   Тип    | Описание                                                                                                                                                                                                                                                                                                     |
|-----------------|:--------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `customElement` |  String  | Если передана не пустая строка - таблица отрендерит слот с таким же именем. В слот можно вставить любые данные или использовать как замену '__slot:actions' (например, если необходимо чтобы столбец с какими-либо кнопками или другими компонентами находился не в самое последней колонке, а любой другой) |
| `format`        | Function | Функция форматирующая значения в столбце                                                                                                                                                                                                                                                                     |
| `icon`          |  String  | Иконка для отображения в каждой строке столбца                                                                                                                                                                                                                                                               | 
| `label`         |  String  | Заголовок столбца                                                                                                                                                                                                                                                                                            |                                                                            
| `name`          |  String  | Используется как ключ по которому определяется какие данные отображать в столбце. Должен соотвествовать какому либо ключу в объекте `data.items`                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                   
| `sortable`      | Boolean  | Определяет можно ли сортировать значения в столбце. Если true - отображается иконка для сортировки                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                 
| `width`         |  String  | Задает уникальную ширину столбца. Иначе используется дефолтная ширина из `data`                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                


3. объект styleConfig: Набор стилей для кастомизации табов.

### Пример styleConfig (в примере указаны дефолтные значения)

```javascript
styleConfig = {
    fontFamily: 'Inter',      // Шрифт
    fontSize: '14px',         // Размер шрифта
    fontColor: '#2D3748',     // Цвет текста
    rowBg: '#718096',         // Цвет строки
    zebraRowBg: '#0066FF',    // Цвет каждой второй строки если template = zebra
    rowHoverBg: '#edf2f7',    // Цвет строки при наведении
    rowSelectBg: '#edf2f7',   // Цвет строки когда она развернута 
    justify: 'flex-start',    // Позиция текста в столбцах 
    borderRadius: '16px',     // Радиус углов таблицы
}
```

### Дополнительные слоты
1. Слот 'loader' - для вставки лоадера при загрузке данных.
   Пример использования:
````vue
<template #loader>
  <ELoader />
</template>
````

2. Слот с именем, переданным в `customElement` - для кастомизации столбца (например, вставить кнопку, виджет, дефолтный текст и т.д.).
   Пример использования:
````vue
<template #HometownNew="{ item, rowIndex }">
  <div>
    HometownNew example {{ item.hometown }} <button>button {{ rowIndex }}</button>
  </div>
</template>
````

3. Слоты 'ItemsPerPage' и 'pagination' - для вставки компонентов для пагинации. Пример использования:
````vue
<template #pagination>
  <Pagination
      class="margin-top"
      :page="currentPage"
      :total-items="totalItems"
      :items-per-page="itemsPerPage"
      @onUpdate="changePage"
      @updateCurrentPage="updateCurrentPage"
  />
</template>

<template #ItemsPerPage
><div class="items-per-page margin-top">
  <label>Items per page</label>

  <ItemsPerPageDropdown
      :list-items-per-page="listItemsPerPage"
      :items-per-page="itemsPerPage"
      @onUpdate="updateItemsPerPage"
  /></div
></template>
````


### События

| Название            | Тип параметров | Описание                                                                                                                                                                                                                                                                                                                                                                           |
|---------------------|:--------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `on-update`         |     Object     | Срабатывает при сортировке. Передает объект с ключами `{sortField, sort, sortCb}`. `sortField` - ключ по которому была сортировка, `sort` - направление сортировки (asc, desc), `sortCb` - коллбэк функция которая сортирует массив, можно использовать свою функцию сортировки. Сортировка данных не происходит внутри компонента `ETable`, для этого передается функция `sortCb` |
| `on-check-all`      |     Array      | Срабатывает при чекбокс в хедере. Передаёт массив всех отмеченных элементов                                                                                                                                                                                                                                                                                                        |
| `on-unchecked-item` |     Object     | Срабатывает при клике на уже отмеченный чекбокс. Передаёт данные этой строки в виде объекта                                                                                                                                                                                                                                                                                        |
| `'on-checked-item`  |     Object     | Срабатывает при клике на чекбокс. Передаёт данные этой строки в виде объекта                                                                                                                                                                                                                                                                                                       |

### Примечания


### Пример использования

````vue
<template>
  <div
    :style="{
      height: '100vh',
    }"
  >
    <ETable
      :data="tableData"
      @onUpdate="onSort"
      @onCheckedItem="onCheckedItem"
      @onUncheckedItem="onUncheckedItem"
      @onCheckAll="onCheckAll"
    >
      <!--      слот для чекбокса -->
      <template #checkbox="checkboxProps">
        <input
          type="checkbox"
          :checked="checkboxProps.checked"
          @click="onCheckboxClick(checkboxProps.click)"
        />
      </template>

      <!--      actions слот с кнопкой  -->
      <template #Label="slotProps">
        <input type="button" class="btn btn-info" value="Edit" @click="onButtonClick(slotProps)" />
      </template>

      <!--      слот для меню с тремя точками -->
      <template #menu="slotProps">
        <div>. . .</div>
      </template>

      <!--      слот для кастомного хедера HometownNew -->
      <template #HometownNew="{ item, rowIndex }">
        <div>
          HometownNew example {{ item.hometown }} <button>button {{ rowIndex }}</button>
        </div>
      </template>

      <!--      слот для содержимого внутри раскрывающейся строки -->
      <template #row-slot="slotPropsExpand">
        <div :style="{ display: 'flex', height: '300px' }">
          {{ slotPropsExpand }}
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusamus corporis error
          explicabo laborum minus, non obcaecati repudiandae sunt tempore voluptatem? Accusantium
          debitis enim expedita quibusdam repellendus sunt tempora ullam, voluptas!
        </div>
      </template>

      <!--       слот для пагинации -->
      <template #pagination>
        <div class="pagination-wrapper">
          <EPagination
            minimalistic-version
            :data="{
              numberOfPages,
              modelValue: currentPage,
              perPage: itemsPerPage,
            }"
            @update:modelValue="changePage"
            @update:perPageValue="updateItemsPerPage"
          />
        </div>
      </template>

      <!--      слот для лоадера -->
      <template #loader>
        <ELoader />
      </template>
    </ETable>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import ELoader from '@/components/animated/ELoader.vue'
import ETable from '@/components/tables/table3/ETable.vue'
import EPagination from '@/components/navigation/EPagination.vue'

export default defineComponent({
  name: 'App',
  components: { ETable, ELoader, EPagination },
  data() {
    return {
      tableData: {
        template: '', // template: 'zebra'
        items: [] as any,
        headerFields: [
          '__slot:expand', // сделать все строки раскрывающимися
          '__slot:checkboxes',
          {
            name: 'firstName',
            label: 'First Name',
            sortable: true,
            icon: 'person', // иконка для всех ячеек в выбранном столбце
          },
          {
            name: 'lastName',
            label: 'Last Name',
            sortable: true,
          },
          {
            name: 'hometown',
            label: 'Hometown',
            sortable: true,
            customElement: 'HometownNew', // задание кастомного хедера (использется слот с именем #HometownNew)
            width: '250px',
          },
          {
            name: 'dob',
            label: 'Data of Birth',
            sortable: true,
          },
          {
            name: 'created',
            label: 'Created',
            sortable: true,
            format: this.formatDate,
          },
          {
            name: 'updated',
            label: 'Updated',
            sortable: false,
            format: this.formatDate,
          },
          '__slot:actions:Label', // слот с именем Label
          '__slot:menu', // слот для меню с тремя точками
        ],
        sortField: 'firstName',
        sort: 'asc',
        notFoundMessage: 'Нет данных',
        trackBy: 'firstName',
        fixedHeight: '600px',
        fixedHeader: true,
        headerCheckbox: true,
        isLoading: false,
        clickableRow: true,
        expandOnIcon: false,
        oneExpand: true,
        useMobileHeader: false,
        menuVisible: false,
        useTransition: true,
      },
      initialData: [
        {
          id: 100,
          firstName: 'Lucca',
          lastName: 'Lin',
          hometown: 'Melbourne',
          dob: '13/02/1975',
          created: new Date().getTime(),
          updated: new Date().getTime(),
          notExpandable: true,
        },
        {
          id: 10,
          firstName: 'Zahid',
          lastName: 'Werner',
          hometown: 'Sydney',
          dob: '18/09/1979',
          created: new Date().getTime(),
          updated: new Date().getTime(),
          notExpandable: true,
        },
        {
          id: 20,
          firstName: 'Gabriel',
          lastName: 'Griffiths',
          hometown: 'Chicago',
          dob: '25/11/1984',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 30,
          firstName: 'Talha',
          lastName: 'Tucker',
          hometown: 'Berlim',
          dob: '27/01/1999',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 40,
          firstName: 'Aariz',
          lastName: 'Piper',
          hometown: 'Auckland',
          dob: '11/07/1964',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 50,
          firstName: 'Macsen',
          lastName: 'Schultz',
          hometown: 'Rio de Janeiro',
          dob: '01/10/1987',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 60,
          firstName: 'Sebastian',
          lastName: 'Cervantes',
          hometown: 'Brisbane',
          dob: '13/11/1994',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 70,
          firstName: 'Tayyab',
          lastName: 'Lister',
          hometown: 'Perth',
          dob: '14/12/1997',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 80,
          firstName: 'Anum',
          lastName: 'Warren',
          hometown: 'Manaus',
          dob: '17/02/1951',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 90,
          firstName: 'Areeba',
          lastName: 'Stein',
          hometown: 'Rome',
          dob: '18/03/1954',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
        {
          id: 5,
          firstName: 'Alesha',
          lastName: 'Sharp',
          hometown: 'New York City',
          dob: '18/04/1966',
          created: new Date().getTime(),
          updated: new Date().getTime(),
        },
      ],
      itemsPerPage: 10,
      currentPage: 1,
      totalItems: 16,
    }
  },
  mounted() {
    this.tableData.items = this.initialData.slice(0, 10)
  },
  computed: {
    numberOfPages(): number {
      return Math.ceil(this.totalItems / this.itemsPerPage)
    },
  },
  methods: {
    // Дополнительная обработка клика по чекбоксу
    onCheckboxClick(checkboxCallback) {
      checkboxCallback()
      console.log('some other actions')
    },
    // клик по чекбоксу в хедере
    onCheckAll(i) {
      console.log('all checked items: ', i)
    },
    // клик по чекбоксу в строке
    onCheckedItem(i) {
      console.log('checked item: ', i)
    },
    // повторный клик по чекбоксу в строке
    onUncheckedItem(i) {
      console.log('unchecked item: ', i)
    },

    // обработка формата даты. Не относится к примеру использования, вспомогательные функции
    addZero(value) {
      return ('0' + value).slice(-2)
    },
    formatDate(value) {
      if (value) {
        const dt = new Date(value)
        return `${this.addZero(dt.getDate())}/${this.addZero(
          dt.getMonth() + 1,
        )}/${dt.getFullYear()}`
      }
      return ''
    },

    // При нажатии на кнопку в слоте
    onButtonClick: (props) => alert('Click props:' + JSON.stringify(props)),

    // При нажатии на иконку сортировки у заголовка
    // sortCb - функция сортировки из компонента ETable
    onSort({ sortField, sort, sortCb }) {
      const sortedData = sortCb(this.initialData, [sortField], sort)
      const start = (this.currentPage - 1) * this.itemsPerPage
      const end = this.currentPage * this.itemsPerPage
      this.tableData.items = sortedData.slice(start, end)
      console.log('load data based on new sort', this.currentPage)
    },

    // Функции для пагинации
    updateItemsPerPage(perPage) {
      this.itemsPerPage = perPage[0].name

      // тут якобы происходит "подгрузка" данных с БД:
      if (perPage >= this.initialData.length) {
        this.tableData.items = this.initialData
      } else {
        this.tableData.items = this.initialData.slice(0, perPage)
      }
      console.log('Загружены данные с новым значением perPage', perPage)
    },

    changePage(currentPage) {
      this.currentPage = currentPage
      const start = (currentPage - 1) * this.itemsPerPage
      const end = currentPage * this.itemsPerPage
      this.tableData.items = this.initialData.slice(start, end)
      console.log('Загружена новая страница данных', currentPage)
    },
  },
  watch: {},
})
</script>

<style lang="scss">
.pagination-wrapper {
  width: 100%;
  display: inline-block;

  margin-bottom: 30px;
}
</style>
````
