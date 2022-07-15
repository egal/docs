# Авторизация

## Модель пользователя

По умолчанию для авторизации используется модель пользователя с полями `email` и `password`.

## Доступные маршруты

Авторизация представлена следующими маршрутами:

### Регистрация

Пример запроса на регистрацию пользователя:
```http request
curl --location --request POST 'http://localhost:80/register' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "admin@gmail.com",
    "password":"123123"
}'
```

### Авторизация

Авторизация основана на двух видах jwt-токенов:

* Токен доступа (AccessToken);
* Токен обновления (RefreshToken).

#### Типы авторизации

С помощью заголовка запроса `Authorization-Type` указывается тип авторизации. Поддерживаемые типы авторизации:
* cookie;

Токены записываются в сессию на сервере, после чего сессия передается в шифрованном виде посредством cookie. В дальнейшем 
для обращения к endpoints в качестве авторизированного пользователя с каждым запросом отправляются полученные cookie.

Пример запроса на вход пользователя с типом авторизации cookie:
```http request
curl --location --request POST 'http://localhost:80/login' \
--header 'Authorization-Type: cookie' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "admin@gmail.com",
    "password":"123123"
}'
```

При успешном входе пользователя в ответ приходит код 200 и в cookie записывается сессия пользователя с токенами.

* header.

Токены передаются в теле ответа при логине пользователя. В дальнейшем для обращения к endpoints в качестве 
авторизированного пользователя с каждым запросом отправляется заголовок `Authorization` с токеном доступа.

Пример запроса на вход пользователя с типом авторизации header:
```http request
curl --location --request POST 'http://localhost:80/login' \
--header 'Authorization-Type: header' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "admin@gmail.com",
    "password":"123123"
}'
```

При успешном входе пользователя в ответ приходит код 200 и json следующего вида:
```json
{
"access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0eXBlIjoiYWNjZXNzIiwiZXhwIjoiMjAyMi0wNy0xNVQwOTo1NTo0Ni4yMjMxMjlaIiwic3ViIjoiNGZhZWRiODQtN2RhMy00NWZmLThkNzktNGQyNjhlNjUzNTQxIiwicm9sZXMiOltdfQ.uLHLolSWS7PrtKapncsoQspzZ21YwJXJSUGqnfJC-NIunsiRTaOGR-1JLXujOKYwIBETY0Q3Rr92YWy5-WSjuOnHF6QMDh-WY_7w_gCqEzFopqwHkZaZUOrOTYJcmRe2prrGr37MCL_7jr_ADsASNkB8AvyG1fuNhI7ocl0tjcw",
"refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0eXBlIjoicmVmcmVzaCIsImV4cCI6IjIwMjItMDctMTVUMDk6NTU6NDYuMjI0ODkzWiIsInN1YiI6IjRmYWVkYjg0LTdkYTMtNDVmZi04ZDc5LTRkMjY4ZTY1MzU0MSJ9.CWYH48UO13nHkvKJTxH8ZeJMWeAQXnmLqeyTS__WqKMypun7xAaU_rl7RzN7MH7q1F_N_QagsKvzHANf3saZj-RFaSlV9LGq99HwSSdPNWmmqRarDy1bXJXOmCwW_pCtTyGCGQLdMDcYY5Ty-YgltcHcUKEOMCTkechGaGy2kPM"
}
```

### Обновление токенов

Для каждого вида токена можно задать время жизни следующими переменными окружения (время в секундах):
* Токен доступа:  `AUTH_ACCESS_TOKEN_TTL`;
* Токен обновления: `AUTH_REFRESH_TOKEN_TTL`;

По истечении срока жизни токена доступа требуется отправить запрос на обновление токена, содержащий токен обновления
в заголовке, либо посредством cookie.

Пример запроса на обновление токена с типом авторизации cookie:
```http request
curl --location --request POST 'http://localhost:80/refreshToken' \
--header 'Authorization-Type: cookie' \
--header 'Cookie: backend_session=eyJpdiI6InVJUGUzeHg3MHZQdXR2TE02SVoyMHc9PSIsInZhbHVlIjoiTG9hYmlMYzV0YVQ4TC9iUVdueTlGdDhKUHROSzFEdldzRUlBQzZRNkNBZ25LNkJTeU4wWjJGYkpvQUs4MUQ4ODdLcEcwUnVxZ0l1SlFuRFhseUZITWZxTWtocXUyMlBKQ2RsSkRiWlE1cWtQN04ydTA2M25sckZZM2piRk1oZTIiLCJtYWMiOiIzOTFmNWQ4NjMxY2RjNjg2NzAyNTE2YzlhZTk1YTZmY2FkZjlhNzAwOTY2NDIzYjJhNWE2NTk0NDRhZTMyODI3IiwidGFnIjoiIn0%3D'
```

При успешном входе пользователя в ответ приходит код 200 и в cookie записывается сессия пользователя с обновленными токенами.

Пример запроса на обновление токенов с типом авторизации header:
```http request
curl --location --request POST 'http://localhost:80/refreshToken' \
--header 'Authorization-Type: header' \
--header 'Refresh-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0eXBlIjoicmVmcmVzaCIsImV4cCI6IjIwMjItMDctMTVUMDk6NTU6NDYuMjI0ODkzWiIsInN1YiI6IjRmYWVkYjg0LTdkYTMtNDVmZi04ZDc5LTRkMjY4ZTY1MzU0MSJ9.CWYH48UO13nHkvKJTxH8ZeJMWeAQXnmLqeyTS__WqKMypun7xAaU_rl7RzN7MH7q1F_N_QagsKvzHANf3saZj-RFaSlV9LGq99HwSSdPNWmmqRarDy1bXJXOmCwW_pCtTyGCGQLdMDcYY5Ty-YgltcHcUKEOMCTkechGaGy2kPM' \
```

При успешном обновлении токена в ответ приходит код 200 и json следующего вида:
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0eXBlIjoiYWNjZXNzIiwiZXhwIjoiMjAyMi0wNy0xNVQxMDo0NTo0OS42MDc3ODFaIiwic3ViIjoiNGZhZWRiODQtN2RhMy00NWZmLThkNzktNGQyNjhlNjUzNTQxIiwicm9sZXMiOltdfQ.VvLmMkTg6_wlzyF7ObSmrbBO0MZENKTkaLfirHXBo7m7H9EVLPenI5u7nQ-H1vPAqRJox5MOAc96PKcuIBYmwy8CU76hVONCsAq7iE9p4ssifF323aTkoD3AogWJd5l4lFMxvCJ3khrwnazenBZ_ETAIgSw52oFHKCUfPplfwcI",
  "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ0eXBlIjoicmVmcmVzaCIsImV4cCI6IjIwMjItMDctMTVUMTA6NDU6NDkuNjE4NTQ1WiIsInN1YiI6IjRmYWVkYjg0LTdkYTMtNDVmZi04ZDc5LTRkMjY4ZTY1MzU0MSJ9.HDuMAtdowyVzM2-nXTHHo75KRghh_W2aNk11jHiM8CDJ5SMwS0mfHvpUy4LzA94un-YCZQhSqOU1o9H_pf43-AtH02G7R_zKGE_KJBuqtvr5KI8P7NXpycaUD0Ho8OY1KKwuvLnj1sTFm0ELI0mptf9kLOyi6RdcNvDFKp12VCA"
}
```
## Модель роли

По умолчанию для работы ролевого доступа используется модель роли с полями `name` и `is_default`. При регистрации
пользователя ему автоматически присваиваются все роли, имеющие значение `is_default` равным true.

## Управление ролями

Для управления ролями поставляются следующие команды:

1. Создание роли:

```shell
php artisan egal:auth:create-role Admin
```
> При необходимости создать роль со значением `is_default`, равным true, следует передать флаг --default.

2. Удаление роли:

```shell
php artisan egal:auth:delete-role Admin
```

3. Вывод списка ролей:

```shell
php artisan egal:auth:list-roles
```
4. Назначение роли пользователю:

```shell
php artisan egal:auth:set-user-role admin@gmail.com Admin
```

5. Удаление роли у пользователя:

```shell
php artisan egal:auth:unset-user-role admin@gmail.com Admin
```
