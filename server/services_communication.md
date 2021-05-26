# Взаимодействие между сервисами

## Требования

Сервисы, которые будут взаимодействовать, регистрируются в сервисе
авторизации.

> Регистрация сервисов пока не обязательна.
<!-- Так нужна или нет регистрация для сервисов? -->

Сервис авторизации должен быть доступен.

Сервис авторизации должен иметь `SERVICE_NAME=auth`.

## Реализация

Пример:

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

