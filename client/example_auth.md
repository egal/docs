# Реализация авторизации

1. Инициализируем экземпляр класса авторизации в компоненте:


   ```javascript
   import { AuthAction } from '@egal/model/compile/index.js';
   import { EventObserver } from '@egal/model/compile/index.js';
   import { Model } from '@egal/model/compile/index';

   const auth = new AuthAction(
       process.env.VUE_APP_USERNAME, // нужен для подключения к rabbitmq
       process.env.VUE_APP_PASSWORD, // нужен для подключения к rabbitmq
       'User',
       'axios'
   );
   ```

2. Устанавливаем url и инициализируем Observer:

   ```javascript
   auth.setBaseURL(process.env.VUE_APP_HTTP_DOMAIN);
   const authObserver = new EventObserver();
   ```

3. Подписываемся на Observer нужной модели и слушаем получаемые им
   события:

   ```javascript
       authObserver.subscribe('User', (data, actionName) => {
       let userData;
       if (data !== 'Start Processing' && actionName !== 'error') {
           switch (actionName) {
               case 'registerByEmailAndPassword': // авторизуем пользователя после успешной регистрации
                   userData = { email: this.email, password: this.password };
                   auth.authUser(userData);
                   break;
               case 'loginByEmailAndPassword': // авторизуем пользователя в нужном микросервисе
                   userData = { service_name: 'monolit', token: data[0] };
                   auth.loginToService(userCred); 
                   break;
               case 'loginToService':
                   // выполняем нужные действия после авторизации пользователя в микросервисе
           }
       } else if (actionName === 'error') {
           this.$toasted.show(data, {
               type: 'error',
               duration: 2000
           });
       }
   });
   ```

