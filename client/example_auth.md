# Реализация авторизации

1. Импортируем класс конструктора авторизации в компонент:

   ```javascript
   import {EgalAuthConstructor} from '@egal/model/compile/index.js';
   ```

2. Инициализируем экземпляр класса и передаем в него информацию, необходимую для его корректной работы:

   ```javascript
   let exampleAuthInfo = {
       modelName: 'exampleModelName', 
       userName: 'admin', 
       password: 'password', 
       url:'exampleUrl', 
       connectionType: 'axios'
   }
   
   let egalAuthExample = new EgalAuthConstructor(exampleAuthInfo)
   ```

3. Используя экземпляр класса вызываем методы Egal в любом нужном месте в компоненте:

- registerNewUser: метод регистрации пользователя.
- authUser: метод аутентификации пользователя.
- loginToService: метод аутентификации пользователя в нужном микросервисе.

Например:

```javascript
function registerNewUser() {
   let userData = {email: 'user email', password: 'user password'}
   egalAuthExample.registerNewUser(userData).then((data) => {
      auth()
   }).catch((error) => {
   })
}

function auth() {
   let userData = {email: 'user email', password: 'user password'}
   egalAuthExample.authUser(userData).then((data) => {
       loginToService(data)
   }).catch((error) => {
   })
}

function loginToService(data: any) {
    let loginData = {service_name: 'microservice name', token: token from data}
    egalAuthExample.loginToService(loginData).then((data) => {
    }).catch((error) => {
    })
}
```

