# Примеры запросов использования действий отдельно от модели:

Для использования действия отдельно от модели инициализируется новый
экземпляр класса с указанием нужных параметров.

Запрос на получение metadata всех моделей приложения:

```javascript
let allMetadata = new GetAllMetaDataAction('auth', 'getAllModelsMetadata', 'ModelManager')
allMetadata.socketConnect()
```

Пример использования `getItems`:

```javascript
let orders = [['name', 'desc'], ['id', 'asc']]

const filter = [
{
  field: 'email',
  operator: 'eq',
  value: 'email@email.com'
},
{
  field: 'id,
  operator: 'eq',
  value: [1,2,3,4,5]
},
{
  left: {
    field: 'age',
    operator: 'gt',
    value: 20
  },
  type: 'or',
  right: {
    field: 'age',
    operator: 'lt',
    value: 30
  }
},
{
  left: {
    field: 'name',
    operator: 'gt',
    value: 'name1'
  },
  type: 'and',
  right: {
    field: 'age',
    operator: 'lt',
    value: 20
  }
}
];

let messageTest = new GetItemsAction('auth', 'User', 'getItems')
messageTest.actionParameters.orders(orders)
messageTest.actionParameters.with(['user', 'roles'])
messageTest.actionParameters.filters(filter)
messageTest.socketConnect()
```

Что происходит в примере:

В переменных `orders` и `filter` хранятся параметры, передаваемые вместе
с запросом. Вид параметров стандартизован и должен быть, таким как
указано.

Массив `orders` хранит массивы с указанием названия сортируемого поля и
порядком сортировки (`asc`: от меньшего к большему, `desc`: от большего
к меньшему).

Массив `filter*` хранит объекты с указанием фильтруемого поля, типа
фильтрации и значения для фильтрации.

Все типы фильтрации указаны [здесь](/server/crud/filters.md)

Когда все параметры определены, вызывается метод `socketConnect()` для
соединения с сервером через сокеты или `axiosConnect()` для соединения
через HTTP запрос.

Запрос определенных полей возможен только с использованием модели.

`getItem`: запрос `getItem` похож на `getItems`, но дополнительно к
параметрам обязательно указывается id сущности с помощью метода
`setId()`

```javascript
messageTest.actionParameters.setId(12)
```

`create`:
```javascript
let createParams = { email: 'test@createParams.com', password: '123456' }
let createAction = new CRUDAction('auth', 'User', 'create', createParams)
createAction.socketConnect()
```

`update`:
```javascript
let updateParams = {id:'id обновляемой записи', email:'test@createParamsUpdated.com'}
let updateAction = new CRUDAction('auth', 'User', 'update', updateParams)
updateAction.socketConnect()
```

`delete`:
```javascript
let deleteParams = { id: 'id удаляемой записи'}
let deleteAction = new CRUDAction('auth', 'User', 'delete', deleteParams)
deleteAction.socketConnect()
```

`createMany`:

```javascript
let createParams = [{email:'test1@createParams1.com', password: '123456'}, {email:'test2@createManyParams2.com', password: '123456'}]
let createAction = new CRUDAction('auth', 'User', 'createMany', createParams)
createAction.socketConnect()
```

`updateMany`:

```javascript
let updateParams = [{id:'', email:'yulya@createParams1Updated.com'}, {id:'', email:'yulya@createManyParams2Updated.com'}]
let updateAction = new CRUDAction('auth', 'User', 'updateMany', updateParams)
updateAction.socketConnect()
```

`deleteMany`:

```javascript
let deleteParams = ['', '', '']
let deleteAction = new CRUDAction('auth', 'User', 'deleteMany', deleteParams)
deleteAction.socketConnect()
```

В параметрах указывается массив *id* удаляемых записей

`updateManyRaw`:

```javascript
let actionFilters = [
   {
       field: 'name',
       type: 'eq',
       value: 'noname1'
   },
   {
       field: 'name',
       type: 'eq',
       value: 'noname2'
   }
]

let paramsUpdateRaw = { attributes: { name: 'yesname' }, filter: [...actionFilters] }
let updateRawAction = new CRUDAction('auth', 'User', 'updateManyRaw', paramsUpdateRaw)
updateRawAction.socketConnect()
```

`deleteManyRaw`:

```javascript
let paramsDeleteRaw = { filter: [...actionFilters] }
let deleteRawAction = new CRUDAction('auth', 'User', 'deleteManyRaw', paramsDeleteRaw)
deleteRawAction.socketConnect()
```


## Пример инициализации модели и ее методов:

1. Инициализируем модель с помощью конструктора ``EgalConstructor``:

```javascript
import { EgalConstructor } from "@egalteam/framework/compile/index";

const exampleParams = {
    modelName: "exampleModelName",
        userName: process.env.VUE_APP_USERNAME,
        password: process.env.VUE_APP_PASSWORD,
        url: process.env.API_BASE_URL,
        connectionType: "axios",
        tokenName: "mandate"
}
this.exampleModelVar = new EgalConstructor(exampleParams)

this.exampleModelVar.initModelObserver().then((data) => {
    // массив data включает в себя всю перечисленную выше информацию
})
```

Описание примера выше можно найти [здесь](/client/start.md)

2. После инициализации модели можно использовать ее методы.

Использование действий, вызываемых через модель (передача параметров
отличается от отдельного использования действий):


`getItems`:

```javascript
let orders = [['name', 'desc'], ['id', 'asc']]
const filter = [
 {
   field: 'email',
   operator: 'eq',
   value: 'email@email.com'
 },
 {
   left: {
     field: 'age',
     operator: 'gt',
     value: 20
   },
   type: 'or',
   right: {
     field: 'age',
     operator: 'lt',
     value: 30
   }
 },
 {
   left: {
     field: 'name',
     operator: 'gt',
     value: 'name1'
   },
   type: 'and',
   right: {
     field: 'age',
     operator: 'lt',
     value: 20
   }
 }
];


let page = 2
let perPage = 25
let withs = ['user', 'roles']

let messageTest = newModel.actionGetItems('auth', 'axios', perPage, page, filter, withs, orders )
```

Что происходит в примере:

В переменных `orders` и `filter` хранятся параметры, передаваемые вместе
с запросом. Вид параметров стандартизирован и должен быть, таким как
указано.

Массив `orders` хранит массивы с указанием названия сортируемого поля и
порядком сортировки (asc: от меньшего к большему, desc: от большего к
меньшему).

Массив `filter` хранит объекты с указанием фильтруемого поля, типа
фильтрации и значения для фильтрации.

Все типы фильтрации указаны [здесь](/server/crud/filters.md)

Вместо вызова метода `socketConnect()` или `axiosConnect()`, в
параметрах указывается тип подключения: `socket` или `axios`.

После запроса, данные сохраняются в модели и их можно вызвать методом
`newModel.getItems()`.

`getItem`:

Запрос getItem похож на getItems, но дополнительно к параметрам
обязательно указывается id перед типом соединения.

```javascript
let messageTest = newModel.actionGetItem('auth', 'axios', id, filter, withs, orders )
```

`create`:

```javascript
let createParams = {email:'test1@createParams1.com', password: '123456'}
let createAction = newModel.actionCreate('auth', 'axios', createParams)
```

`update`:

```javascript
let updateParams = {id:'', email:'yulya@createParams1Updated.com'}
let updateAction = newModel.actionUpdate('auth', 'axios', updateParams)
```

`delete`:

```javascript
let deleteParams = [id]
let actionDelete = newModel.actionDelete('auth', 'axios', deleteParams)
```

`createMany`:

```javascript
let createParams = [{email:'yulya@createParams1.com', password: '123456'}, {email:'yulya@createManyParams2.com', password: '123456'}]
let createManyAction = newModel.actionCreateMany('auth', 'axios', createParams)
```

`updateMany`:

```javascript
let updateParams = [{id:'', email:'yulya@createParams1Updated.com'}, {id:'', email:'yulya@createManyParams2Updated.com'}]

let updateManyAction = newModel.actionUpdateMany('auth', 'axios', updateParams)
```

`deleteMany`:

```javascript
let deleteManyParams = ['', '', '']
let deleteManyAction = newModel.actionDeleteMany('auth', 'axios', deleteManyParams)
```

В параметрах указывается массив *id* удаляемых записей.


`updateManyRaw`:

```javascript
let actionFilters = [{
   field: 'name',
   type: 'eq',
   value: 'noname1'
},
   {
       field: 'name',
       type: 'eq',
       value: 'noname2'
   }]
let paramsDeleteRaw = { filter: [...actionFilters] }
let updateRaw = newModel.actionUpdateManyWithFilter('auth', 'axios', paramsUpdateRaw)
```

`deleteManyRaw`:

```javascript
let paramsDeleteManyRaw = {filter:[...actionFilters]}
let deleteRaw = newModel.actionDeleteManyWithFilter('auth', 'axios', paramsDeleteManyRaw)
```

`actionCustom`:

```javascript
let paramsCustomAction = {...}
let customAction = newModel.actionCustom('auth', 'anyCustomEndpoint', 'axios', paramsCustomAction)
```

Запрос на получение metadata модели

```javascript
let metadata = newModel.actionGetMetadata('auth', 'axios')
```

