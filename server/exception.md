# Исключения

## Формирование ответа при возникновении исключения

В случае выбрасывания исключений сервисом HTTP-код ответа устанавливается атрибутом `code` типа integer класса `Ecxeption` и в теле ответа
содержится сообщение, формируемое атрибутом `message` класса `Exception`. 

Рассмотрим на примере исключения `TokenExpiredException`:
```php
class TokenExpiredException extends Exception
{

    protected $message = 'Token expired!';
    protected $code = 401;

}
```
При возникновении такого исключения HTTP-код ответа будет 401 и тело ответа будет выглядеть следующим образом:
```json
{
  "action": {
    "type": "action",
    "service_name": "core",
    "model_name": "User",
    "action_name": "test",
    "parameters": {
      "attributes": {
        "name": "test",
        "surname": "test"
      }
    },
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidXN0IiwiYXV0aF9pbmZvcm1hdGlvbiI6eyJpZCI6ImNhN2YxMmQxLTk3YTItNGM1OS04OTJiLWUzMTUyNWMxZTQyMiIsImVtYWlsIjoiaXZhbkBtYWlsLnJ1IiwiY3JlYXRlZF9hdCI6IjIwMjEtMTEtMDhUMDQ6NDc6MzQuMDAwMDAwWiIsInVwZGF0ZWRfYXQiOiIyMDIxLTExLTA4VDA0OjQ3OjM0LjAwMDAwMFoiLCJhdXRoX2lkZW50aWZpY2F0aW9uIjoiY2E3ZjEyZDEtOTdhMi00YzU5LTg5MmItZTMxNTI1YzFlNDIyIiwicm9sZXMiOltdLCJwZXJtaXNzaW9ucyI6W119LCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDhUMDU6MTk6MjEuNjY5MDQwWiJ9.damKsf-fZLJu4nNryA-n7_fDKSqMCGVZMREEuEHTGw4",
    "uuid": "73b8ce09-20a8-4be1-bd63-d0e12ae9e3f3"
  },
  "start_processing": {
    "type": "start_processing",
    "started_at": "2021-11-08T06:28:55.074825Z",
    "uuid": "20c2a425-83ea-42c9-a300-dd123f683a59",
    "action_message": {
      "type": "action",
      "service_name": "core",
      "model_name": "User",
      "action_name": "test",
      "parameters": {
        "attributes": {
          "name": "test",
          "surname": "test"
        }
      },
      "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidXN0IiwiYXV0aF9pbmZvcm1hdGlvbiI6eyJpZCI6ImNhN2YxMmQxLTk3YTItNGM1OS04OTJiLWUzMTUyNWMxZTQyMiIsImVtYWlsIjoiaXZhbkBtYWlsLnJ1IiwiY3JlYXRlZF9hdCI6IjIwMjEtMTEtMDhUMDQ6NDc6MzQuMDAwMDAwWiIsInVwZGF0ZWRfYXQiOiIyMDIxLTExLTA4VDA0OjQ3OjM0LjAwMDAwMFoiLCJhdXRoX2lkZW50aWZpY2F0aW9uIjoiY2E3ZjEyZDEtOTdhMi00YzU5LTg5MmItZTMxNTI1YzFlNDIyIiwicm9sZXMiOltdLCJwZXJtaXNzaW9ucyI6W119LCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDhUMDU6MTk6MjEuNjY5MDQwWiJ9.damKsf-fZLJu4nNryA-n7_fDKSqMCGVZMREEuEHTGw4",
      "uuid": "73b8ce09-20a8-4be1-bd63-d0e12ae9e3f3"
    }
  },
  "action_result": null,
  "action_error": {
    "type": "action_error",
    "code": 401,
    "message": "Token expired!",
    "uuid": "3cbed69d-c7e7-471a-805d-61a554f2255e",
    "action_message": {
      "type": "action",
      "service_name": "core",
      "model_name": "User",
      "action_name": "test",
      "parameters": {
        "attributes": {
          "name": "test",
          "surname": "test"
        }
      },
      "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0eXBlIjoidXN0IiwiYXV0aF9pbmZvcm1hdGlvbiI6eyJpZCI6ImNhN2YxMmQxLTk3YTItNGM1OS04OTJiLWUzMTUyNWMxZTQyMiIsImVtYWlsIjoiaXZhbkBtYWlsLnJ1IiwiY3JlYXRlZF9hdCI6IjIwMjEtMTEtMDhUMDQ6NDc6MzQuMDAwMDAwWiIsInVwZGF0ZWRfYXQiOiIyMDIxLTExLTA4VDA0OjQ3OjM0LjAwMDAwMFoiLCJhdXRoX2lkZW50aWZpY2F0aW9uIjoiY2E3ZjEyZDEtOTdhMi00YzU5LTg5MmItZTMxNTI1YzFlNDIyIiwicm9sZXMiOltdLCJwZXJtaXNzaW9ucyI6W119LCJhbGl2ZV91bnRpbCI6IjIwMjEtMTEtMDhUMDU6MTk6MjEuNjY5MDQwWiJ9.damKsf-fZLJu4nNryA-n7_fDKSqMCGVZMREEuEHTGw4",
      "uuid": "73b8ce09-20a8-4be1-bd63-d0e12ae9e3f3"
    }
  },
  "error_message": "Token expired!"
}
```
## Внутренний код исключения

Фреймворк предоставляем возможность создавать исключения с внутренним кодом, наследуясь от поставляемого класса `InternalException`.
Внутренний код - строковый атрибут `internalCode`, по которому фронтенд-разработчики могут идентифицировать конкретную ошибку. В связи с этим
внутренние коды исключений должны быть уникальными и неизменяемыми. Атрибут `internalCode`  класса `Ecxeption` будет отображаться в теле ответа.

Рассмотрим пример следующего исключения:

```php
class FooInternalException extends InternalException
{

    protected string $internalCode = 'foo';
    protected $message = 'Long message with error\'s description!';
    protected $code = 401;
    
}
```
>
> Если нет возможности использовать наследование класса `InternalException`, можно использовать интерфейс `HasInternalCode`.

При возникновении такого исключения тело ответа будет выглядеть следующим образом:
```json
{
  "action": {
    "type": "action",
    "service_name": "core",
    "model_name": "User",
    "action_name": "test",
    "parameters": {
      "attributes": {
        "name": "test",
        "surname": "test"
      }
    },
    "token": null,
    "uuid": "d2d62fb9-589a-4033-869d-e82fbee3c57e"
  },
  "start_processing": {
    "type": "start_processing",
    "started_at": "2021-11-08T07:02:33.462170Z",
    "uuid": "60e7ea29-cee4-400e-a20d-a5cfc831790b",
    "action_message": {
      "type": "action",
      "service_name": "core",
      "model_name": "User",
      "action_name": "test",
      "parameters": {
        "attributes": {
          "name": "test",
          "surname": "test"
        }
      },
      "token": null,
      "uuid": "d2d62fb9-589a-4033-869d-e82fbee3c57e"
    }
  },
  "action_result": null,
  "action_error": {
    "code": 401,
    "message": "Long message with error's description!",
    "internal_code": "foo",
    "type": "action_error",
    "uuid": "c549c36f-875f-4a93-b328-40145e16cad3",
    "action_message": {
      "type": "action",
      "service_name": "core",
      "model_name": "User",
      "action_name": "test",
      "parameters": {
        "attributes": {
          "name": "test",
          "surname": "test"
        }
      },
      "token": null,
      "uuid": "d2d62fb9-589a-4033-869d-e82fbee3c57e"
    }
  },
  "error_message": "Long message with error's description!"
}
```

## Формирование ответа при возникновении исключения с интерфейсом HasObjects
В коробке Egal доступен подключаемый интерфейс HasObjects. Он позволяет добавлять конкретные объекты, для которых сработало исключение.
Пример класса исключения с интерфейсом HasObjects:
```php
class ObjectException extends Exception implements HasObjects
{
    protected $code = 500;
    protected $message = 'test message';

    public function getObjects(): array
    {
        return [
          'key' => 'value'
        ];
    }
}
```

Если интерфейс подключен, ответ при возникновении исключения будет содержать список объектов, и тело ответа будет выглядеть следующим образом:
 ```json
{
 "action": {
    "type": "action",
    "service_name": "first",
    "model_name": "Model",
    "action_name": "ping",
    "parameters": [],
    "token": null,
    "uuid": "e26d0a98-54a4-4d30-8d4e-7be820976338"
  },
  "action_result": null,
  "action_error": {
    "code": 500,
    "message": "test message",
    "internal_code": null,
    "data": {
      "key": "value"
    },
    "type": "action_error",
    "uuid": "34a750e1-58a1-46dc-895a-5784724b2935"
  }
}
```
