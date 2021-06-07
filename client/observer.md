# Использование Observer внутри проекта в качестве транслятора кастомных событий:

Класс Observer можно использовать непосредственно внутри приложения для
осуществления внутренних операций.

Рассмотрим на примере создания всплывающих уведомлений.

1. Создадим соответствующий класс:

```javascript
export class Toaster {

// инициализируем в нем Observer

    observer = new EventObserver()

// создадим нужный для класса метод

    success(message: string): void {
        const params = {
            message,
            variant: 'success'
        }
        this.observer.broadcast(params, 'toaster', 'toaster')
    }
}
```

2. В компоненте, при вызове нужного метода, инициализируем экземпляр
   класса Toaster и вызываем его метод success():

```javascript

activateAlert() {
    cosnt toaster = new Toaster()
    toaster.success('message')
}

```

3. В нужном компоненте подписываемся на события Toaster

```javascript

mounted() {
    const observer = new EventObserver()
    observer.subscribe('toaster', (data:any) => {
        // выполняем нужные действия
    })
}

```

