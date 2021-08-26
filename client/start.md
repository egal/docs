## Начало работы

Установка пакета возможна с помощью команды:

```
npm i @egalteam/framework
```

Для отслеживания изменений модели Egal использует Observer.
Observer инициализируется для каждой модели отдельно и получает события,
предназначенные для этой модели.

Для того, чтобы начать использовать Egal в каком-либо компоненте проекта нужно:

1. Импортировать класс ``EgalConstructor``
```javascript
import { EgalConstructor } from "@egalteam/framework/compile/index";
```

2. Инициализировать модель с помощью ``EgalConstructor``

```javascript
this.exampleModelVar = new EgalConstructor(exampleParams)
```

В качестве параметра он принимает объект с нужной для инициализации информацией:

```javascript
exampleParams: {
        modelName: "exampleModelName",
        userName: process.env.VUE_APP_USERNAME,
        password: process.env.VUE_APP_PASSWORD,
        url: process.env.API_BASE_URL,
        connectionType: "axios",
        tokenName: "mandate"
}
```

3. После инициализации экземпляра класса конструктора, нужно вызвать его метод получения данных ``initModelObserver()``.
После запроса ответ сервера, название вызванного экшена и название модели будут возвращаться в него:
```javascript
    this.exampleModelVar.initModelObserver().then((data) => {
        // массив data включает в себя всю перечисленную выше информацию
})
```

4. Далее можно пользоваться любыми методами модели в любом месте компонента. 
Полный список доступных методов можно найти [здесь](/client/model.md)


Стартовые шаблоны для проектов, использующих Vue или Nuxt можно найти по ссылкам:
1. [Vue](https://github.com/egal/egal-vue-project).
2. [Nuxt](https://github.com/egal/egal-nuxt-project).