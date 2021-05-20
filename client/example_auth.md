## Подключение авторизации:

1. Импортируются нужные классы:
```
import {AuthAction} from "@egal/model/compile/build/index.js";
import {EventObserver} from "@egal/model/compile/build/index"
```
- инициализируется новый объект модели, принимающий в себя параметры:
    - username: требуется для установки подключения к rabbitmq, определяется в .env файле;
    - password: требуется для установки подключения к rabbitmq, определяется в .env файле;
    - имя модели;
    - способ подключения (_сейчас доступен только axios, позже можно будет использовать сокеты_);
    - и устанавливается нужный url;
```
const auth = new AuthAction('User', 'axios')
auth.setBaseURL(host.authUrl)
```

2. Метод авторизации выглядит следующим образом:
```
auth() {
   let userAuth = {email: this.email, password: this.password}
   auth.authUser(userAuth)
}
```
3. Метод регистрации следующим:
```
createAccount() {
   let userReg = {email: this.email, password: this.password}
   auth.registerNewUser(userReg)
}
```
4. Observer:
```
let observer = new EventObserver()
observer.subscribe('User', (data, actionName) => {
    if (data !== 'Start Processing' && actionName !== 'error') {
        switch (actionName) {
            case 'registerByEmailAndPassword':
                let userData = {email: this.email, password: this.password}
                auth.authUser(userData);
            break;
            case 'loginByEmailAndPassword':
                let userCred = {service_name: 'monolit', token: data[0]}
                auth.loginToService(userCred)
            break;
            case 'loginToService':
                // some user action
        }
    } else if (actionName === 'error') {
        this.errorAlert(data)
    }
})
```
