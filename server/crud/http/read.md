| Метод | URI                                       |
|:-----:|:------------------------------------------|
|  GET  | `/[ServiceName]/[ModelName]/getItems`     |
|  GET  | `/[ServiceName]/[ModelName]/getItem/[id]` |

Пример запроса на получение списка работников
`domain/Service/Employee/getItems`

Запросы на чтение поддерживают дополнительные возможности:

JSON payload

```json
{
  "per_page": 5,
  "page": 2,
  "filter": [
    ["full_name", "eq", "Иванов Иван Иванович"]
  ],
  "withs": [
    "departments"
  ],
  "order": [
    {"column": "id", "direction": "asc"},
    {"column": "email", "direction": "desc"}
  ]
}
```

где

`per_page` - количество записей на 1 страницу

`page` - текущая страница записей

`filter` - фильтрация по полям (см.
[Фильтры](/server/crud/filters.md))

`order` - сортировка по полям

`withs` - связанные сущности (relations)

