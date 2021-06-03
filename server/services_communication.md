# Общение между сервисами

## Требования

Сервисы, которые будут общаться, должны быть зарегистрированы в сервисе авторизации

> Регистрация сервисов пока не обязательна.
<!-- Так нужна или нет регистрация для сервисов? -->

Сервис авторизации должен быть доступен.

Подразумевается что сервис авторизации должен иметь `SERVICE_NAME=auth`. 
В случае если имя сервиса авторизации отличается от значения по умолчанию, его можно задать путем
передачи параметра в конструктор.

```php
$request = new \Egal\Core\Communication\Request(
    'first', // Сервис назначения запроса
    'Model', // К какой модели обращение
    'test', // К какому действию обращение
    [], // Параметры действия
    false, // Отключена ли авторизация
    'auth-service' // Имя сервиса авторизации
);
```

Перед отправкой запроса нужно убедиться, что в модели сервиса получателя прописаны доступы до endpoint.
Их можно указать через тэг `@services-access` и `@statuses-access`.

```php
/**
 * @action test {@statuses-access guest}
 * @action checks {@statuses-access logged} {@services-access auth,monolit}
 */
class Model extends EgalModel
{
    public static function actionTest() {/*...*/}
    public static function actionChecks() {/*...*/}
}
```

## Доступ

При отправке запроса генерируются токены smt и sst для получения доступа к определенному действию.

Вы также можете сгенерировать установить свой токен с помощью метода `setToken()`.

## Реализация

Пример с отправкой запроса и получением ответа:

```php
// В сервисе отправителя запроса
$request = new \Egal\Core\Communication\Request(
    'first', // Сервис назначения запроса
    'Model', // Обращение к модели
    'test', // Обращение к действию
    [] // Параметры действия
);
$request->call(); // Отправка и ожидание ответа
$response = $request->getResponse();
if ($response->hasError()) {
    $actionErrorMessage = $response->getActionErrorMessage(); // Получение сообщения ошибки
} else {
    $actionResultMessage = $response->getActionResultMessage(); // Получение сообщения результата выполнения действия
}
```

Пример с большим контролем процесса запроса и получения ответа:

```php
// В сервисе отправителе запроса
$request = new \Egal\Core\Communication\Request(
    'first', // Сервис назначения запроса
    'Model', // Обращение к модели
    'test', // Обращение к действию
    [] // Параметры действия
);
if (!$request->isConnectionOpened) {
    $request->openConnection(); // Открытие подключения
}
$request->publish(); // Публикация сообщения запроса
$request->waitReplyMessages(); // Ожидание ответа
$request->closeConnection(); // Закрытие подключения
$response = $request->getResponse();
if ($response->hasError()) {
    $actionErrorMessage = $response->getActionErrorMessage(); // Получение сообщения ошибки
} else {
    $actionResultMessage = $response->getActionResultMessage(); // Получение сообщения результата выполнения действия
}
```

Пример с отправкой запроса без ожидания ответа:

```php
// Где-то в сервисе отправителе запроса
$request = new \Egal\Core\Communication\Request(
    'first', // Сервис назначения запроса
    'Model', // К какой модели обращение
    'test', // К какому действию обращение
    [] // Параметры действия
);
if (!$request->isConnectionOpened) {
    $request->openConnection(); // Открытие подключения
}
$request->send(); // Отправка сообщения запроса
$request->closeConnection(); // Закрытие подключения
```
