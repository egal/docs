# Способы запуска/остановки сервиса

## Как запустить сервис

* Запуск сервиса:

  ```shell
  docker-compose up -d [SERVICE_NAME]
  ```

  > Где SERVICE_NAME - название сервиса Docker Compose, который является
  > сервисом проекта.

* Запуск клонов контейнера сервиса — см.
  [Docker Compose Scale](https://docs.docker.com/compose/reference/scale/).


## Как остановить сервис

* Остановка сервиса:

  ```shell
  docker-compose stop [SERVICE_NAME]
  ```

  > Где SERVICE_NAME - название сервиса Docker Compose, который является
  > сервисом проекта.


## Как запустить слушателя сервиса

* Первый способ:

    Запускает слушателей внутри контейнера по ранее предоставленным CMD
    инструкциям.

    ```shell
    docker-compose up -d [SERVICE_NAME]
    ```

    > Где SERVICE_NAME - название сервиса Docker Compose, который является
    > сервисом проекта.


* Второй способ:

    ```shell
    docker-compose run --rm [SERVICE_NAME] ./artisan egal:listener:run
    ```

    > Где SERVICE_NAME - название сервиса Docker Compose, который является
    > сервисом проекта.

* Третий способ:

    ```shell
    docker-compose exec [SERVICE_NAME] ./artisan egal:listener:run
    ```

    > Где SERVICE_NAME - название сервиса Docker Compose, который является
    > сервисом проекта.

* Четвертый способ:

    ```shell
    docker exec -ti [SERVICE_CONTAINER_NAME] ./artisan egal:listener:run
    ```

    > Где SERVICE_CONTAINER_NAME - название контейнера Docker, который
    > является сервисом проекта.

* Пятый способ:

    Зайдите в контейнер и выполните команду:

    ```shell
    ./artisan egal:listener:run
    ```


### Как остановить слушателя сервиса

* Первый способ:

    Если процесс запущен в реальном времени в текущей сессии терминала —
    просто остановите выполнение команды.

* Второй способ:

    Остановка всех слушателей внутри контейнера:

    ```shell
    docker-compose stop [SERVICE_NAME]
    ```

    > Где SERVICE_NAME - название сервиса Docker Compose, который является
    > сервисом проекта.


* Третий способ

    ```shell
    kill [PID]
    ```

    > Где PID - идентификатор процесса слушателя.

    > Получение списка процесса описано ниже.


## Как отследить кол-во запущенных слушателей

1. Проверяем количество контейнеров сервиса:

    ```shell
    docker-compose ps
    ```

    Если их больше одного, то все следующие действия и советы необходимо
    применять для всех контейнеров.

    > В некоторых случаях понадобится посмотреть все контейнеры системы:
    >
    > ```shell
    > docker ps
    > ```

2. Проверяем количество процессов слушателей в контейнере

    * Первый способ:

        ```shell
        docker-compose exec [SERVICE_NAME] ps ax | grep listener
        ```

        > Где SERVICE_NAME - название сервиса Docker Compose, который
        > является сервисом проекта.

    * Второй способ:

        ```shell
        docker exec -ti [SERVICE_CONTAINER_NAME] ps ax | grep listener
        ```

        > Где SERVICE_CONTAINER_NAME - название контейнера Docker, который
        > является сервисом проекта.

    * Третий способ:

        ```shell
        ps ax | grep listener
        ```

        > ВНИМАНИЕ! В данном способе выведется список всех слушателей всех
        > сервисов всех проектов текущей системы (то есть список процессов в
        > названиях которых есть слово `listener`)

        После получения списка слушателей можно выборочно остановить их:

        ```shell
        kill [PID]
        ```

        > Где PID - идентификатор процесса слушателя.

