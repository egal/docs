## Action Constructor

С помощью Action Constructor можно использовать асинхронные запросы к серверу без инициализации модели.
Конструктор содержит следующие методы:

```javascript
  getMetadata(microserviceName, modelName)

  getItems(microserviceName, modelName)

  getItem(microserviceName, modelName, id)

  create(microserviceName, modelName, actionParams)

  update(microserviceName, modelName, actionParams)

  delete(microserviceName, modelName, actionParams)

  createMany(microserviceName, modelName, actionParams)

  updateMany(microserviceName, modelName, actionParams)

  updateManyWithFilter(microserviceName, modelName, actionParams)

  deleteManyWithFilter(microserviceName, modelName, actionParams)

  custom(microserviceName, modelName, actionName, actionParams)

  getCount(microserviceName, modelName)

  filter(filterObject)

  withs(withs)

  order(orders)

  setPagination(perPage, page)

```

Для использования конструктора нужно инициализировать экземпляр класса (глобально либо в нужном компоненте):
```javascript
import { ActionConstructor } from '@egalteam/framework'
let constructor = new ActionConstructor(url)
```

Вызвать какой-либо из методов можно так, передав нужные параметры в каждый из них.
Методы в цепочке могут быть вызваны в произвольном порядке, однако, последним должен быть вызван метод `call()`,
который в последствии вернет ответ сервера с помощью Promise.

```javascript
constructor.getItems(microserviceName, modelName).filter(filters).order(orders).withs(withs).call()
  .then((data) => {
    
  }).catch((error) => {
    
  })
```