# Gitlab CI

# Ветвление

В Egal можно использовать как встроенную систему ветвления - One Code
Base Workflow (OCB Workflow), так и
[настроить ее самостоятельно](#Как-настроить-свой-git-workflow).

OCB Workflow совмещает следующие системы ветвления:
* Central Workflow
* Feature Branch Workflow
* Issue Branch Workflow


## Как с этим работать

1. Создаем репозиторий с основной веткой (main, master и т.д.).
2. Создаем Feature либо Issue ветки, там реализуем функционал.
3. При готовности функционала — делаем Pull в основную ветку.
4. Ставим версию стабильности `beta`.
5. Теперь можем залить на develop.
6. Ставим версию стабильности `RC`.
7. Теперь можем залить на stage.
8. Ставим версию стабильности `release`.
9. Теперь можем залить на production.

> Примеры версии стабильности `beta`: `v1.0.0beta1`, `v2.1.0beta11`,
> `v0.12.1beta5`.
>
> Примеры версии стабильности `RC` (Release Candidate): `v1.0.0RC1`,
> `v2.1.0RC11`, `v0.12.1RC5`.
>
> Примеры версии стабильности `release`: `v1.0.0`, `v2.1.0`, `v0.12.1`.
>
> То есть до версии `v1.0.0` желательно выпустить версии `v1.0.0RC1` и
> `v1.0.0beta1`. Версий `beta` и `RC` может быть несколько.


## Преимущества использования

Так как современные приложения состоят из множества репозиториев, нужно
максимально упростить Git Workflow для экономии времени.

При единой кодовой базе однообразны и все сборки проекта на всех версиях
серверов. Это позволяет экономить время на проверке одинаковых функций
на разных серверах.

В этой схеме build приложения делается один раз на все типы серверов,
что оптимизирует общее время deploy, и позволяет не задумываться о
разных окружениях частей приложения на разных серверах.


# Структура develop репозитория:

```bash
develop # Repository
├── deploy # Repository
│   ├── .gitlab-ci.yml # Gitlab CI файл (содержание и описание смотри ниже)
│   ├── docker-compose.deploy.yml # Deploy Compose файл, дополняющий общий Compose файл для Develop, Stage и Production версий приложения.
│   ├── docker-compose.develop.yml # Develop Compose файл, дополняющий общий Compose файл для Develop версии приложения.
│   ├── docker-compose.production.yml # Production Compose файл, дополняющий общий Compose файл для Production версии приложения.
│   ├── docker-compose.stage.yml # Stage Compose файл, дополняющий общий Compose файл для Stage версии приложения.
│   ├── docker-compose.yml # Общий Compose файл конфигурирует сервисы для всех окружений.
│   └── proxy
│       ├── Dockerfile # Используется build proxy
│       ├── ... # Дополнительные файлы, требующиеся для build proxy
│       └── nginx.conf
├── first-service # Service Repository
│   ├── .gitlab-ci.yml # Gitlab CI файл (содержание и описание смотри ниже)
│   ├── Dockerfile # Файл build сервиса, который должен полностью подготовить контейнер, который при развороте запустит сервис.
│   └── ... # Другие файлы сервиса
├── second-service # Service Repository
│   ├── .gitlab-ci.yml # Gitlab CI файл (содержание и описание смотри ниже)
│   ├── Dockerfile # Файл build сервиса, который должен полностью подготовить контейнер, который при развороте запустит сервис.
│   └── ... # Другие файлы сервиса
├── third-service # Service Repository
│   ├── .gitlab-ci.yml # Gitlab CI файл (содержание и описание смотри ниже)
│   ├── Dockerfile # Файл build сервиса, который должен полностью подготовить контейнер, который при развороте запустит сервис.
│   └── ... # Другие файлы сервиса
├── docker-compose.local.yml # Local Compose файл, дополняющий общий Compose файл для Local версии приложения.
└── ... # Другие файлы
```


# Содержание файла `deploy/.gitlab-ci.yml`

```yaml
variables:
  PROJECT_NAME: project
  CONTAINER_REGISTRY: harbor
  HARBOR_URL: domain
  HARBOR_USER: user
  HARBOR_PASSWORD: password

include:
  - remote: "URL"
```


# Содержание файла `{some}-service/.gitlab-ci.yml`

```yaml
stages:
  - build
  - test:before_deploy
  - delivery
  - deploy
  - test:after_deploy

variables:
  PROJECT_NAME: project
  CONTAINER_REGISTRY: harbor
  HARBOR_URL: domain

include:
  - remote: "URL" # Всегда должен быть первым
  - remote: "URL" # Всегда должен быть перед любой стадией
  - remote: "URL" # Всегда должен быть до сервисов
  - remote: "URL" # Всегда должен быть до сервисов
  - remote: "URL"
  - remote: "URL"
  - remote: "URL"
  - remote: "URL" # Всегда должен быть в конце
```


# Переменные окружения для всех `.gitlab-ci.yml`:


## `PROJECT_NAME`

Базовое название проекта.

Например: egal, telegram-bot, geos.


## `CONTAINER_REGISTRY`

Какой реестр контейнеров использовать.

Один из списка: harbor.

> Если указан `CONTAINER_REGISTRY: harbor` - необходимо указать
> дополнительные переменные окружения: HARBOR_URL - URL без `http://` и
> `https://`, HARBOR_USER - пользователь, HARBOR_PASSWORD - пароль
> пользователя.


# Дополнение CI/CD

Можно создавать, дополнять и переопределять задачи и стадии.

Для понимания решения задачи, необходимо погрузиться в `includes`, и
ознакомится с текущей базой Gitlab CI.

Примеры:

```yaml
# Переопределение стадии deploy develop, определяя запуск pipeline по commit в master ветку.
.develop:stage:template:
  only:
    - master
```

```yaml
# Переопределение стадии proxy deploy develop, выставляя дополнительные переменные окружения.
proxy:deploy:develop:
  variables:
    PROXY_PORT: 7543
```

```yaml
# Дополнение CI заданием deploy monolit-service.
.monolit-service:deploy:template:
  stage: deploy
  when: manual
  script:
    - docker-compose up -d --force-recreate monolit-service;

monolit-service:deploy:develop:
  extends:
    - .develop:stage:template
    - .monolit-service:deploy:template

monolit-service:deploy:stage:
  extends:
    - .stage:stage:template
    - .monolit-service:deploy:template

monolit-service:deploy:production:
  extends:
    - .production:stage:template
    - .monolit-service:deploy:template
```


# Как настроить свой Git Workflow

Пример перехода на Gitflow Workflow:

```yaml
# Определяем ветку develop, для deploy в develop
.develop:stage:template:
  only:
    - develop

# Определяем ветку stage, для deploy в stage
.stage:stage:template:
  only:
    - stage

# Определяем ветку production, для deploy в production
.production:stage:template:
  only:
    - production
```

# Рекомендации по настройке пайплайнов

При деплое с использованием Gitlab Runners необходимо всегда отключать настройку `Skip outdated deployment jobs` в разделе
`CI/CD Settings/General pipelines` во избежание возникновения ошибки:
>The deployment job is older than the previously succeeded deployment job, and therefore cannot be run 


