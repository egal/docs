# Разворот контейнера с Centrifugo

Пример определения Centrifugo в docker-compose.yml:
```php
    centrifugo:
    image: centrifugo/centrifugo:v3
    restart: always
    ulimits:
      nofile:
        soft: "65536"
        hard: "65536"
    environment:
      CENTRIFUGO_TOKEN_HMAC_SECRET_KEY: secret
      CENTRIFUGO_ADMIN_PASSWORD: admin_password
      CENTRIFUGO_API_KEY: api_key
      CENTRIFUGO_ADMIN_SECRET: admin_secret
      CENTRIFUGO_WEB: 1
      CENTRIFUGO_ADMIN: 1
      CENTRIFUGO_DEBUG: 1
      CENTRIFUGO_CONNECTION_LIFETIME: 0
      CENTRIFUGO_WATCH: 0
      CENTRIFUGO_CLIENT_INSECURE: 1
      CENTRIFUGO_ADMIN_INSECURE: 0
      CENTRIFUGO_API_INSECURE: 0
      CENTRIFUGO_PUBLISH: 1
      CENTRIFUGO_PRESENCE: 0
      CENTRIFUGO_JOIN_LEAVE: 0
      CENTRIFUGO_HISTORY_SIZE: 10
      CENTRIFUGO_HISTORY_TTL: 300s
      CENTRIFUGO_HISTORY_RECOVER: 1
      CENTRIFUGO_ANONYMOUS: 0
    ports:
      - "8000:8000"
```
Пример определения конфигураций для подключения сервиса к Centrifugo в docker-compose.yml:
```php
  core-service:
    environment:
      CENTRIFUGO_SECRET: secret
      CENTRIFUGO_API_KEY: api_key
      CENTRIFUGO_API_URL: http://centrifugo:8000/api
```