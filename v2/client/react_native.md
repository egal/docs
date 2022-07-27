# Использование Egal в приложениях React Native
Для использования Egal в приложениях React Native необходимо установить [отдельный пакет](https://www.npmjs.com/package/@egalteam/framework-react-native) с помощью команды:
```
npm i @egalteam/framework-react-native
```

Основной пакет при этом устанавливать не нужно.

Инициализация и работа с моделями выполняется тем же образом, что и при использовании базового пакета Egal.

## Пример интеграции авторизации с Redux

Для интеграции авторизации с Redux необходимо выполнить следующие действия:

1. Импортируем `EgalAuthConstructor` и `setCookie`:

```typescript
import { EgalAuthConstructor } from '@egalteam/framework-react-native/index';
```
```typescript
import { setCookie } from '@egalteam/framework-react-native/src/GlobalVariables';
```

2. Инициализируем модель с помощью `EgalAuthConstructor`:

```typescript
const authModel = new EgalAuthConstructor(userModelData);
```

- EgalAuthConstructor принимает объект, содержащий имя модели и базовый домен:

```typescript
const userModelData = {
  modelName: 'User',
  url: BASE_API_URL,
};
```

4. Оборачиваем методы авторизации из `EgalAuthConstructor` в промисы, например:
```typescript
export const auth = (userData: UserDataType) =>
  new Promise((resolve, reject) => {
    authModel
      .authUser(userData)
      .then((data: any) => {
        loginToService(data[0]).then((resp) => {
          resolve(resp);
        });
      })
      .catch((error) => {
        reject(error);
      });
  });
```
- при желании можно выделить методы в отдельный helper и экспортировать методы оттуда

5. Теперь в экшнах Redux можно вызывать `auth` асинхронно:
```typescript
export const loginThunkAction =
  (userData: UserDataType) =>
  async (dispatch: Dispatch<ActionTypes>): Promise<void> => {
    dispatch(loginRequestAction());
    await auth(userData) // асинхронный вызов auth
      .then((resp: any) => {
        dispatch(loginSuccessAction(resp));
      })
      .catch(() => {
        dispatch(loginFailAction('Error'));
      });
  };
```
