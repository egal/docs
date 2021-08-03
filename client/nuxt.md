# Использование Egal в приложениях Nuxt

Использование Egal в приложениях Nuxt 
отличается от использования в приложениях Vue.

В шаблон [egal nuxt project](https://github.com/egal/egal-nuxt-project) 
уже включены необходимые для правильной конфигурации файлы:

1) Класс `EgalConstructor` используется для инициализации модели и принимает в себя следующие параметры:
  - `modelName`: название модели
  - `emitName`: название события для `observer` (задается произвольно)
  - `$root`: доступ к корневому экземпляру компонента, в котором инициализирована модель
  - `listenerFunction`: название функции, принимающей полученные данные в компоненте, в котором инициализирована модель

Для того чтобы инициализировать модель в компоненте нужно:
1) Импортировать класс `EgalConstructor` в нужный компонент
2) В хуке `mounted` определить новый экземпляр этого класса и функцию, ответственную за получение данных из `observer`:
3) Вызвать у модели метод `emitModelEvent()` для инициализации `observer`
```javascript
mounted() {
    let exampleFunction = (data) => {}
    this.egalModel = new ModelConstructor('ExampleModelName', 'example-event-name', this.$root, exampleFunction)
    this.egalModel.emitModelEvent()
}
```
4) В хуке `beforeDestroy` вызвать у инициализированной модели метод `unsubscribe()`

```javascript
beforeDestroy() {
    this.egalModel.unsubscribe()
}
```

Далее, инициализированную модель можно использовать в методах компонента.
