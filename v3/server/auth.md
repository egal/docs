# Авторизация

####   
Авторизация основана на двух видах jwt-токенов:

* Токен доступа (AccessToken);
* Токен обновления (RefreshToken).


#### Время жизни токенов (TTL)

Для каждого вида токена можно задать время жизни следующими переменными
окружения (значение означает время в секундах):