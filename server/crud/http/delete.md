| Метод  | URI                                        |
|:------:|:-------------------------------------------|
| DELETE | `/[ServiceName]/[ModelName]/delete`        |
| DELETE | `/[ServiceName]/[ModelName]/deleteMany`    |
| DELETE | `/[ServiceName]/[ModelName]/deleteManyRaw` |

Пример удаления работника `domain/Service/Employee/delete`

JSON payload для delete:

```json
{
  "id": 1
}
```

JSON payload для deleteMany:

```json
{
    "ids": [1, 2, 3]
}
```

JSON payload для deleteManyRaw:

```json
{
  "filter": [
    ["status", "co", "fired"]
  ]
}
```
