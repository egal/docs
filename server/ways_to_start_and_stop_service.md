# Способы запуска/остановки сервиса

## Как запустить сервис

* Запуск сервиса:

  ```shell
  docker-compose up -d [SERVICE_NAME]
  ```

  > Где SERVICE_NAME - название сервиса проекта Docker Compose.
   
* Запуск клонов контейнера сервиса

  Командой [Docker Compose Scale](https://docs.docker.com/compose/reference/scale/).

## Как остановить сервис

* Остановка сервиса:

  ```shell
  docker-compose stop [SERVICE_NAME]
  ```

  > Где SERVICE_NAME - название сервиса проекта Docker Compose.

## Как запустить слушателя сервиса

* Первый способ:

Запуск слушателей внутри контейнера по CMD инструкциям.
  ```shell
  docker-compose up -d [SERVICE_NAME]
  ```

  > Где SERVICE_NAME - название сервиса проекта Docker Compose.


* Второй способ:

  ```shell
  docker-compose run --rm [SERVICE_NAME] ./artisan egal:listener:run
  ```

  > Где SERVICE_NAME - название сервиса проекта Docker Compose.

* Третий способ:

  ```shell
  docker-compose exec [SERVICE_NAME] ./artisan egal:listener:run
  ```

  > Где SERVICE_NAME - название сервиса проекта Docker Compose.

* Четвертый способ:

  ```shell
  docker exec -ti [SERVICE_CONTAINER_NAME] ./artisan egal:listener:run
  ```

  > Где SERVICE_CONTAINER_NAME - название сервиса проекта Docker Compose.
  
* Пятый способ:
  
  Выполнение команды в контейнере:
  ```shell
  ./artisan egal:listener:run
  ```

<!-- Есть пять разных способ запуска слушателя сервиса. Может стоит пояснить когда каждый из них используется/какой из них лучше? -->

### Как остановить слушателя сервиса

* Первый способ:

  Если процесс запущен в реальном времени в текущей сессии терминала — через остановку выполнения команды.

* Второй способ:

 Остановка всех слушателей внутрни контейнера:
  ```shell
  docker-compose stop [SERVICE_NAME]
  ```

  > Где SERVICE_NAME - название сервиса проекта Docker Compose.


* Третий способ

  ```shell
  kill [PID]
  ```

  > Где PID - идентификатор процесса слушателя.
  
  > Получение списка процесса описано ниже.

## Как отследить кол-во запущенных слушателей

* 1. Проверить количество контейнеров сервиса:

    ```shell
    docker-compose ps
    ```
    
    Если их больше одного, то все следующие действия
    и советы необходимо применять для всех контейнеров.
    
    > В некоторых случаях понадобится посмотреть все контейнеры системы:
    > ```shell
    > docker ps
    > ```

* 2. Проверка количества процессов слушателей в контейнере

  * Первый способ:
    
      ```shell
      docker-compose exec [SERVICE_NAME] ps ax | grep listener
      ```
  
    > Где SERVICE_NAME - название сервиса проекта Docker Compose.

  * Второй способ:

    ```shell
    docker exec -ti [SERVICE_CONTAINER_NAME] ps ax | grep listener
    ```
<!-- Вставить расшифровку SERVICE_CONTAINER_NAME? -->
  * Третий способ:

    ```shell
    ps ax | grep listener
    ```

    > ВНИМАНИЕ! В данном способе выведется список 
    > всех слушателей всех сервисов всех проектов текущей системы (т.е. список процессов в названиях которых есть слово `listener`)
  
    После получения списка слушателей можно выборочно остановить их:
  
    ```shell
    kill [PID]
    ```
  
    > Где PID - идентификатор процесса слушателя.

  
 