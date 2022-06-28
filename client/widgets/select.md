## Select

Определяется тэгом:
```vue
<e-select></e-select>
```

### Параметры
Селект принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр                        |       Тип       |   По умолчанию   | Возможные значения                                                                           | Описание                                                                                                                                                                                                                                                  |
|---------------------------------|:---------------:|:----------------:|----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ``paceholder``                  |     String      |      ``''``      | Любая строка                                                                                 | Placeholder                                                                                                                                                                                                                                               |
| ``label``                       |     String      |      ``''``      | Любая строка                                                                                 | Label                                                                                                                                                                                                                                                     |
| ``helperText``                  |     String      |      ``''``      | Любая строка                                                                                 | Мелкий текст под селектом                                                                                                                                                                                                                                 |
| ``error``                       |     String      |      ``''``      | Любая строка                                                                                 | Вывод кастомной ошибки                                                                                                                                                                                                                                    |
| ``modelValue``                  | [Object, Array] |      ``[]``      | Любой массив или объект с выбранным(и) из списка значением(ями) (можно использовать v-model) | Значение(я) селекта (можно использовать v-model)                                                                                                                                                                                                          |
| ``options``                     |      Array      |      ``[]``      | Массив объектов возможных значений                                                           | Список возможных значений                                                                                                                                                                                                                                 |
| ``shownKey``                    |     String      |     ``name``     | Любая строка                                                                                 | Ключ, по которому выводить значение в селект и дропдаун                                                                                                                                                                                                   |
| ``validators``                  |      Array      |      ``[]``      | Массив функций-валидаторов                                                                   | Проверка значения будет проходить через каждую функцию-валидатор                                                                                                                                                                                          |
| ``size``                        |     String      |      ``md``      | ``sm``, ``md``, ``lg``                                                                       | Размер селекта                                                                                                                                                                                                                                            |
| ``showError``                   |     Boolean     |     ``true``     | ``true``, ``false``                                                                          | Показ ошибки и состояния селекта                                                                                                                                                                                                                          |
| ``clearable``                   |     Boolean     |     ``true``     | ``true``, ``false``                                                                          | Возможность удаления введенного значения с помощью кнопки                                                                                                                                                                                                 |
| ``multiple``                    |     Boolean     |    ``false``     | ``true``, ``false``                                                                          | Возможность выбора более одного значения из списка                                                                                                                                                                                                        |
| ``searchable``                  |     Boolean     |    ``false``     | ``true``, ``false``                                                                          | Осуществление поиска по списку значений                                                                                                                                                                                                                   |
| ``searchableInput``             |     Boolean     |    ``false``     | ``true``, ``false``                                                                          | Отображение инпута с поиском по дропдауну                                                                                                                                                                                                                 |
| ``searchPlaceholder``           |     String      |    ``Search``    | Любая строка                                                                                 | Для изменения дефолтного текста в инпуте с поиском в дропдауне                                                                                                                                                                                            |
| ``emptyDropdownText``           |     String      |   ``no data``    | Любая строка                                                                                 | Для текста если в дропдауне нет опций или ничего не найдено по поиску                                                                                                                                                                                     |
| ``grouped``                     |     Boolean     |    ``false``     | ``true``, ``false``                                                                          | Группировка списока в дропдауне (нужен определенный вид options)                                                                                                                                                                                          |
| ``isLocalOptions``              |     Boolean     |     ``true``     | ``true``, ``false``                                                                          | Если `data.isLocalOptions: false`, фильтрация по локальным опциям в дропдауне не происходит, можно передавать в `data.options` ответ с отфильтрованными данными с бэка                                                                                    |
| ``dropdownPosition``            |     String      |    ``bottom``    | ``bottom``, ``top``                                                                          | Установка позиционирования списка опций                                                                                                                                                                                                                   |
| ``showMoreButtonDisplay``       |     Boolean     |    ``false``     | ``true``, ``false``                                                                          | Нужно ли отображать кнопку "show more". Если этот параметр равен `false` - другие 2 параметра (`nonLocalOptionsTotalCount` и `showMoreButtonText`) ни на что не влияют                                                                                    |
| ``closeDropdownAfterSelection`` |     Boolean     |     ``true``     | ``true``, ``false``                                                                          | Если closeDropdownAfterSelection = true (дефолтное значение), закрывает дропдаун после выбора опции, иначе - оставляет дропдаун открытым. В мультиселекте значение closeDropdownAfterSelection игнорируется, мультиселект всегда закрывается по клику вне |
| ``nonLocalOptionsTotalCount``   |     Number      |      ``0``       | Любое число                                                                                  | Принимает общее количество всех опций (поле total_count из ответа getItems). На основе этого значения рассчитывает, нужно ли продложать отрисовывать кнпоку "show more" в конце списка или все опции уже отображены                                       |
| ``showMoreButtonText``          |     String      | ``Show more...`` | Любая строка                                                                                 | Текст кнопки                                                                                                                                                                                                                                              |
| ``dropdownStyleConfig``         |     Object      |      ``{}``      | Объект стилей                                                                                | Кастомизация стилей дропдауна                                                                                                                                                                                                                             |
| ``inputSearchStyleConfig``      |     Object      |      ``{}``      | Объект стилей                                                                                | Кастомизация стилей инпута поиска                                                                                                                                                                                                                         |
| ``showFilled``                  |     Boolean     |    ``false``     |                                                                                              |                                                                                                                                                                                                                                                           |
| `helperText`                    |     String      |       `''`       | Любая строка                                                                                 | Мелкий текст под селектом/инпутом селекта                                                                                                                                                                                                                 |
| `shownBadgeKey`                 |     String      |       `''`       | Любая строка                                                                                 | Ключ, по которому выводить значение (количество чего-либо) в бейджике для каждого элемента в дропдауне. Работает аналогично `shownKey`                                                                                                                    |
| `showSuccess`                   |     Boolean     |     `false`      | `true`, `false`                                                                              | Показывать "успешное" состояние инпута                                                                                                                                                                                                                    |
| `inputConfig`                   |     Объект      |       `{}`       | Объект, передаваемый как `data` для EInput                                                   | Должен содержать в себе параметры из `data` объекта для EInput, передается в инпуты с поиском для кастомизации                                                                                                                                            |


- Если у селекта флаги `multiple` и `searchableInput` равны `true` - в инпуте будут появляться теги. Вписанные в инпут теги добавляются по нажатию на Enter.
- В ESelect можно передавать инлайн стили `:chips-style-config` для кастомизации "тегов".
- В ESelect в `data` можно передавать икноки для инпута (т.е. также как для EInput: `iconLeft`, `iconRight`)
2. объект `styleConfig`:
Набор стилей для кастомизация селекта.

### События
| Название              | Тип параметров | Описание                             |
|-----------------------|:--------------:|--------------------------------------|
| ``error``             |     String     | Возвращает ошибку из валидаторов     |
| ``update:modelValue`` |     String     | Возвращает выбранное(ые) значение(я) |

#### Пример использования
```vue
# Пример использования
````vue
<template>
  <div :style="{ width: '280px', marginTop: '20px' }">
    <ESelect
      :data="{ ...selectdata }"
      :style-config="{
        fontFamily: 'Inter',
      }"
      @update:modelValue="upd"
      @show-more="addMore"
      @input="
        ($event) => {
          searchOptions($event.target.value)
        }
      "
      @error="fff"
    ></ESelect>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'App',
  data() {
    return {
      selectdata: {
        multiple: true,
        modelValue: [],
        options: [
          {
            name: '1',
            amount: 22,
          },
          { name: '2', amount: 2 },
          { name: '3' },
          {
            name: '4',
            amount: 22,
          },
          { name: '5', amount: 2 },
          { name: '6' },
        ],
        // isLocalOptions: false,
        nonLocalOptionsTotalCount: 20,
        showMoreButtonDisplay: true,
        showMoreButtonText: 'Показать больше',
        validators: [this.required],
        closeDropdownAfterSelection: false,
        closeDropdown: false,
        // dropdownPosition: 'top',
        iconLeft: 'archive',
        iconRight: 'archive',
        shownBadgeKey: 'amount',
        // disabled: true,
        label: 'Default label',
        helperText: 'helper',
        // error: true,
        // showSuccess: true,
        // openDropdown: true,
        dropdownStyleConfig: {
          activeBackgroundColor: 'red',
          activeHoverBackgroundColor: '#f55'
        },
      },
    }
  },
  methods: {
    required(value) {
      return !Object.keys(value).length ? 'Обязательное поле' : ''
    },
    
    upd(v) {
      this.selectdata.modelValue = v
      this.selectdata.closeDropdown = true
    },
    searchOptions(v) {
      this.selectdata.options = this.selectdata.options.filter((i) => i.name.includes(v))
    },
  },
})
</script>
<style lang="scss">
.a {
  display: grid;
  grid-row-gap: 30px;
}
</style>
````


#### Пример styleConfig
````javascript
styleConfig = {
        fontFamily: 'Inter',
        valueColor: 'black',
        valueFontWeight: '500',
        placeholderColor: 'gray',
        placeholderFontSize: '12px',
        labelColor: 'black',
        labelFontWeight: '400',
        labelFontSize: '12px',
        helperTextColor: 'gray',
        helperTextFontWeight: '400',
        helperTextFontSize: '10px',
        borderColor: 'gray',
        borderRadius: '6px',
        backgroundColor: 'white',
        focusBorderColor: 'blue',
        filledBackgroundColor: 'yellow',
        filledFontColor: 'blue',
        crossColor: 'gray',
        arrowColor: 'gray',
        errorColor: 'red',
      }
````

#### Пример dropdownStyleConfig
````javascript
dropdownStyleConfig = {
        fontFamily: 'Open Sans',
        optionColor: 'black',
        optionHoverBackgroundColor: 'gray',
        optionFontWeight: 500,
        activeBackgroundColor: 'blue',
        activeOptionColor: 'white',
        groupNameColor: 'gray',
        groupNameFontWeight: 600,
        backgroundColor: 'white',
        borderColor: 'blue',
        borderRadius: '10px',
        boxShadow: '0px 0px 1px rgba(12, 26, 75, 0.24), 0px 3px 8px -1px rgba(50, 50, 71, 0.05)',
        optionPressBackgroundColor: 'Цвет фона при нажатии на опцию',
        activeHoverBackgroundColor: 'Цвет фона при наведении на выбранную опцию',
        activePressBackgroundColor: 'Цвет фона при нажатии на выбранную опцию'
      }
````