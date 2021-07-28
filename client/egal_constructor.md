# Egal Constructor

Класс Egal Constructor - это альтернативный способ использовать egal.
Вместо того чтобы инициализировать Model и EventObserver отдельно, 
можно воспользоваться Egal Constructor.


#### Egal Constructor инициализируется следующим образом, в нужном компоненте или миксине:

```javascript
    let model
    modelParams: {
         modelName: string,
         userName: string,
         password: string,
         url: string,
         connectionType: string
    }
    model = new EgalConstructor(this.modelParams)
    model.initModelObserver().then((data) => {
      // любое нужное действие
    })
```