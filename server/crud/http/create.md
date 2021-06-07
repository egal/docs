| Метод | URI                                     |
|:-----:|:----------------------------------------|
| POST  | `/[ServiceName]/[ModelName]/create`     |
| POST  | `/[ServiceName]/[ModelName]/createMany` |

Пример создания нового работника `domain/Service/Employee/create`

JSON payload для create:

```json
{
  "attributes": {
    "full_name": "Иванов Иван Иванович",
    "email": "ivanov_ivan@domain.com"
  }
}
```

JSON payload для createMany:

```json
{
  "objects": [
    {
      "full_name": "Иванов Иван Иванович",
      "email": "ivanov_ivan@domain.com"
    },
    {
      "full_name": "Петров Петр Петрович",
      "email": "petrov_petr@domain.com"
    }
  ]
}
```
