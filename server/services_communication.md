# Общение между сервисами

## Требования

Сервисы, которые будут общаться, должны быть зарегистрированы в сервисе
авторизации (см. [документацию](/server/authorization.md)).

Сервис авторизации должен быть доступен.

```php
$request = new \Egal\Core\Communication\Request(
    'first', // Сервис назначения запроса
    'Model', // К какой модели обращение
    'test', // К какому действию обращение
    ['attributes' => ['name' => 'test 1']], // Параметры действия
);
```

Перед отправкой запроса нужно убедиться, что в модели сервиса получателя
прописаны доступы до endpoint. Их можно указать через тэг
`@services-access` и `@statuses-access`.

```php
/**
 * @action test {@statuses-access guest}
 * @action checks {@statuses-access logged} {@services-access auth|monolit}
 */
class Model extends EgalModel
{
    public static function actionTest(array $attributes = []) {/*...*/}
    public static function actionChecks() {/*...*/}
}
```


## Доступ

При отправке запроса генерируются токены SMT и SST для получения доступа
к определенному действию.

Вы также можете сгенерировать установить свой токен с помощью метода
`setToken()`.

## Настройка времени ожидания ответа от другого сервиса

В случае, когда время выполнения полученного запроса сервисом превышает 10 секунд,
необходимо для сервиса, отправляющего запросы, установить в секундах конфигурацию `APP_REQUEST_WAIT_REPLY_MESSAGE_TTL`
в docker-compose.yml:

```yaml
core-service:
  ...
  environment:
    APP_NAME: ${PROJECT_NAME}
    ...
    APP_REQUEST_WAIT_REPLY_MESSAGE_TTL: 20
```

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
   if ($response->getStatusCode() != 200) {
       $actionErrorMessage = $response->getActionErrorMessage(); // Получение сообщения ошибки
   } else {
       $actionResultMessage = $response->getActionResultMessage(); // Получение сообщения результата выполнения [действия](/_glossary?id=действия) }
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

$request->send(); // Отправка сообщения запроса
```

