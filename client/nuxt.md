# Использование Egal в приложениях Nuxt

Использование Egal в приложениях Nuxt 
отличается от использования в приложениях Vue.

В шаблон [egal nuxt project](https://github.com/egal/egal-nuxt-project) 
уже включены необходимые для правильной конфигурации файлы:

1) Плагин `observer` инициализирует универсальный observer, единый для всего приложения.
Он доступен в любом месте приложения.
   
2) Класс `EgalConstructor` используется для инициализации модели и принимает в себя следующие параметры:
  - `modelName`: название модели
  - `emitName`: название события для `observer` (задается произвольно)
  - `$root`: доступ к корневому экземпляру компонента, в котором инициализирована модель
  - `listenerFunction`: название функции, принимающей полученные данные в компоненте, в котором инициализирована модель

Для того чтобы инициализировать модель в компоненте нужно:
1) Импортировать класс `EgalConstructor` в нужный компонент
2) В хуке `mounted` определить новый экземпляр этого класса и функцию, ответственную за получение данных из `observer`:

```javascript
mounted() {
    let exampleFunction = (data) => {}
    this.egalModel = new ModelConstructor('ExampleModelName', 'example-event-name', this.$root, exampleFunction)
}
```

3) В хуке `asyncData` инициализировать `observer` для выбранной модели:

```javascript
  asyncData(context) {
    context.app.$observer('ExampleModelName', 'example-event-name')
  }
```

Далее, инициализированную модель можно использовать в методах компонента.