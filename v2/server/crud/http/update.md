| Меread.md |                                            |
|:---------:|:-------------------------------------------|
|    PUT    | `/[ServiceName]/[ModelName]/update`        |
|    PUT    | `/[ServiceName]/[ModelName]/updateMany`    |
|    PUT    | `/[ServiceName]/[ModelName]/updateManyRaw` |

Пример обновления сущности работник `domain/Service/Employee/update`

JSON payload для update:

```json
{
  "id": "ea806608-71bb-4279-84f1-6d8473b9076c",
  "attributes": {
    "full_name": "Иванов Иван Иванович",
    "email": "ivanov_ivan@domain.com"
  }
}
```

JSON payload для updateMany:

```json
{
  "objects": [
    {
      "id": "ea806608-71bb-4279-84f1-6d8473b9076c",
      "full_name": "Иванов Иван Иванович",
      "email": "ivanov_ivan@domain.com"
    },
    {
      "id": "1a8e6608-71bb-4279-84f1-6d8473b9076c",
      "full_name": "Петров Петр Петрович",
      "email": "petrov_petr@domain.com"
    }
  ]
}
```

JSON payload для updateManyRaw:

```json
{
  "filter": [
    ["email", "eq", "ivanov_ivan@domain.com"]
  ],
  "attributes": {
    "name": "Иванов Иван Иванович"
  }
}
```
