## Calendar

Определяется тэгом:
```vue
<e-calendar></e-calendar>
```

Принимает  2 параметра в качестве пропа:
1. объект data, содержимое описано в таблице ниже

### Параметры объекта data
| Параметр              |   Тип   | Возможные значения                                                                                              | Описание                                                                                                                                                     |
|-----------------------|:-------:|:----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ``isDouble``          | Boolean | ``true``, ``false``                                                                                             | Отображать ли "двойной" календарь (по умолчанию ``false``)                                                                                                   |
| ``locale``            | String  | ``'en-US', 'ru-RU'``                                                                                            | Заданная локаль влияет на язык, на котором будут отображаться названия дней недели и месяца. По умолчанию - `en-US`                                          |
| ``timePicker``        | Object  |                                                                                                                 | Объект, позволяющий управлять форматом и лейблом селекта/селектов для выбора времени                                                                         |
| ``timePicker.isAMPM`` | Boolean | ``true``, ``false``                                                                                             | Отображать время в 12-часовом или 24-часовом формате. Если значение - ``true``, время отображается в 12-часовом формате (добавляется селект с выбором AM/PM) | 
| ``timePicker.label``  | String  | Любая строка                                                                                                    | Текст лейбла над селектом/селектами для выбора времени                                                                                                       |
| ``data``              | Object  |                                                                                                                 | Объект с данными (позволяет заранее установить начальную и конечную даты)                                                                                    |
| ``data.date_from``    | String  | Дата или дата и время в формате ISO (``"2022-03-02T04:28:12.089Z"`` или ``"2022-03-02"``, если время не задано) | Начальная дата (если выбран диапазон) или выбранная                                                                                                          |
| ``data.date_to``      | String  | Дата в формате ISO                                                                                              | Размер инпута                                                                    Конечная дата                                                               |
| ``inputData``         | Object  |                                                                                                                 | Объект, передаваемый в инпут как проп :data, для настройки инпута                                                                                            |
| ``showInput``         | Boolean | ``true``, ``false``                                                                                             | Определяет, отображать ли инпут для выбора даты и времени над календарем                                                                                     |
| ``isRange``           | Boolean | ``true``, ``false``                                                                                             | Определяет, можно ли выбрать диапазон дат, вместо одной. Если ``isDouble = true``, по умолчанию всегда можно выбрать диапазон                                |
| ``minutesIncrement``  | Number  | `1`, `5`, `10`...                                                                                               | Устанавливает шаг для отображения минут в селекте. По умолчанию - 1                                                                                          |
| `yearSelectRange`     | Object  | `{ min: undefined, max: 2100 }`                                                                                 | Задает диапазон значений для выбора года в селекте в календаре. Если min: undefined, подставляется текущий год как минимальное значение для выбора.          |


2. объект styleConfig: Набор стилей для кастомизация календаря.

### Пример styleConfig (в примере указаны дефолтные значения)
````javascript
styleConfig = {
  fontFamily: 'Open Sans',
  fontWeight: 'normal',
  activeColor: '#0066FF', // цвет выбранной даты
  activeBackgroundColor: '#E5F0FF', // цвет при наведении на дату в календаре
  currentDayBorderColor: '#b3d1ff',
  fontSize: '14px'
}
````

### События
| Название               | Тип параметров | Описание                                                                                                                  |
|------------------------|:--------------:|---------------------------------------------------------------------------------------------------------------------------|
| ``update:dateValue``   |     String     | Возвращает массив выбранных дат и времени                                                                                 |
| ``close``              |     String     | Срабатывает при закрытии календаря                                                                                        |
| ``open``               |     String     | Срабатывает при открытии календаря                                                                                        |


#### Пример использования
````vue
<template>
  <div :style="{ width: '300px' }">
    <Calendar
        :data="calendarData"
        :style-config="{
        fontFamily: 'Raleway',
        fontSize: '14px',
        fontWeight: 'regular',
        activeColor: '#0066FF',
        activeBackgroundColor: '#E5F0FF',
      }"
        @update:dateValue="(v) => setDate(v)"
        @onError:inputValue="(errorMsg) => handleError(errorMsg)"
    ></Calendar>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import Calendar from '@/components/calendar/Calendar.vue'
export default defineComponent({
  name: 'App',
  components: { Calendar },
  data() {
    return {
      calendarData: {
        isExpanded: false,
        isDouble: false,
        isRange: false,
        showInput: true,
        locale: 'en-US',
        timePicker: {
          isAMPM: false,
        },
        date: {
          date_from: "2022-03-02",
          date_to: "2022-03-05"
        },
        // можно передавать параметры для инпута с датой
        // возможные передаваемые параметры описаны в документации по EInput, объект :data
        inputData: {
          size: 'md',
          showFilled: false,
          validators: [this.required],
        },
        // можно передавать параметры для селекта со временем
        // возможные передаваемые параметры описаны в документации по ESelect, объект :data
        timeSelectData: {
          showFilled: false,
        },
      },
    }
  },
  methods: {
    required(value) {
      return !value ? 'Обязательное поле' : ''
    },
    handleError(error) {
      // ...
    },
    setDate() {
      // ...
    },
  },
})
</script>
````