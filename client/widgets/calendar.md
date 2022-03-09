## Calendar

Определяется тэгом:
```vue
<e-calendar></e-calendar>
```

Принимает 1 объект data, содержимое описано в таблице ниже

### Параметры объекта data
| Параметр                  |       Тип        | Возможные значения                                                                                              | Описание                                                                                                                                                     |
|---------------------------|:----------------:|:----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ``fontFamily``            |      String      | Любая строка                                                                                                    | font-family текста                                                                                                                                           |
| ``fontWeight``            | [String, Number] | ``'regular'``, ``800``                                                                                          | font-weight текста                                                                                                                                           |
| ``fontSize``              |      String      | ``'14px'``                                                                                                      | font-size текста                                                                                                                                             |                                                     
| ``activeColor``           |      String      | ``"#0066FF"``                                                                                                   | Цвет фона выбранного дня (или первого и последнего дня, если выбран диапазон дней)                                                                           |                                                 
| ``activeBackgroundColor`` |      String      | ``"#E5F0FF"``                                                                                                   | Цвет фона дней внутри выбранного диапазона, также цвет фона при наведении                                                                                    |                                                          
| ``isAdaptiveSize``        |     Boolean      | ``true``, ``false``                                                                                             | Должен ли календарь растягиваться по ширине контейнера. Еслм значение - ``false``, календарь будет иметь дефолтные ширину и высоту                           | 
| ``isDouble``              |     Boolean      | ``true``, ``false``                                                                                             | Отображать ли "двойной" календарь                                                                                                                            |
| ``locale``                |      String      | ``'en-US'``                                                                                                     | Заданная локаль влияет на язык, на котором будут отображаться названия дней недели и месяца                                                                  |
| ``timePicker``            |      Object      |                                                                                                                 | Объект с параметрами для инпута с выбором времени                                                                                                            |
| ``timePicker.isAMPM``     |     Boolean      | ``true``, ``false``                                                                                             | Отображать время в 12-часовом или 24-часовом формате. Если значение - ``true``, время отображается в 12-часовом формате (добавляется селект с выбором AM/PM) | 
| ``timePicker.label``      |      String      | Любая строка                                                                                                    | Текст лейбла над селектом                                                                                                                                    |
| ``data``                  |      Object      |                                                                                                                 | Объект с данными (выбранные даты)                                                                                                                            |
| ``data.date_from``        |      String      | Дата или дата и время в формате ISO (``"2022-03-02T04:28:12.089Z"`` или ``"2022-03-02"``, если время не задано) | Начальная дата (если выбран диапазон) или выбранная                                                                                                          |
| ``data.date_to``          |      String      | Дата или дата и время в формате ISO                                                                             | Размер инпута                                                                    Конечная дата                                                               |            


### События
| Название             |  Тип параметров   | Описание                                  |
|----------------------|:-----------------:|-------------------------------------------|
| ``update:dateValue`` |      String       | Возвращает массив выбранных дат и времени |


#### Пример использования
```vue
<div v-for="(item, idx) in jsonData" :key="idx" style="margin-bottom: 40px">
    <Calendar :data="item" @update:dateValue="setDate" />
</div>
```
 
#### Пример объекта data
```json
[
  {
    "fontFamily": "Raleway",
    "fontSize": "14px",
    "fontWeight": "regular",
    "activeColor": "#0066FF",
    "activeBackgroundColor": "#E5F0FF",
    "isAdaptiveSize": false,
    "isDouble": true,
    "locale": "ru-RU",
    "timePicker": {
      "isAMPM": true,
      "label": "time 1"
    },
    "data": {
      "date_from": "2022-03-02T04:28:12.089Z",
      "date_to": "2022-03-05"
    }
  },
  {
    "fontFamily": "Raleway",
    "fontSize":  "14px",
    "fontWeight": 500,
    "activeColor": "#51e",
    "activeBackgroundColor": "#E5F0FF",
    "isAdaptiveSize": false,
    "isDouble": false,
    "locale":  "en-US",
    "timePicker": {
      "isAMPM": false,
      "label":  "time 2"
    },
    "data": {
      "date_from": "2022-03-02"
    }
  },
  {
    "fontFamily": "Open Sans",
    "fontSize":  "18px",
    "activeColor": "red",
    "activeBackgroundColor": "#ffF0FF",
    "isAdaptiveSize": false,
    "data": {}
  }
]
```