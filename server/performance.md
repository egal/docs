# Производительность

В данном разделе приведены результаты оценки производительности Egal под нагрузкой
с использованием инструментов Siege и k6 для моделирования нагрузки.

---

**Параметры нагрузки**
* Время в секундах: 30 
* Количество пользователей: 25

---

**Характеристики используемой машины**
* Процессор: Intel(R) Core(TM) i7-8565U CPU @ 1.80GHz
* Оперативная память: 16G

---

**Тестируемый проект**
* web-service: 
  * Количество: 1
  * image: egalbox/web-service:v2.0.0
* first-service:
    * Количество: 1
    * Версия egal/framework: v2.0.0
* HTTP-запрос: `http://localhost/first/Model/ping`

---
**Результаты**

Результат запуска скрипта с Siege (2333 t/s)

```
** SIEGE 4.0.4
** Preparing 25 concurrent users for battle.
The server is now under siege...
Lifting the server siege...
Transactions:                  69920 hits
Availability:                 100.00 %
Elapsed time:                  29.97 secs
Data transferred:              19.27 MB
Response time:                  0.01 secs
Transaction rate:            2333.00 trans/sec
Throughput:                     0.64 MB/sec
Concurrency:                   24.86
Successful transactions:       69920
Failed transactions:               0
Longest transaction:            8.47
Shortest transaction:           0.00
```

Результат запуска скрипта с k6 (2130 t/s)

```
running (0m30.0s), 00/25 VUs, 63950 complete and 0 interrupted iterations
default ✓ [======================================] 25 VUs  30s

     data_received..................: 31 MB  1.0 MB/s
     data_sent......................: 5.8 MB 194 kB/s
     http_req_blocked...............: avg=3.56µs  min=1.41µs  med=3.15µs  max=3.27ms  p(90)=3.62µs  p(95)=4.25µs 
     http_req_connecting............: avg=200ns   min=0s      med=0s      max=1.07ms  p(90)=0s      p(95)=0s     
     http_req_duration..............: avg=11.63ms min=2.64ms  med=10.84ms max=93.23ms p(90)=15.38ms p(95)=17.22ms
       { expected_response:true }...: avg=11.63ms min=2.64ms  med=10.84ms max=93.23ms p(90)=15.38ms p(95)=17.22ms
     http_req_failed................: 0.00%  ✓ 0           ✗ 63950
     http_req_receiving.............: avg=53.88µs min=18.53µs med=50.08µs max=12.58ms p(90)=63.63µs p(95)=69.42µs
     http_req_sending...............: avg=16.06µs min=7.13µs  med=14.36µs max=14.2ms  p(90)=20.14µs p(95)=22.17µs
     http_req_tls_handshaking.......: avg=0s      min=0s      med=0s      max=0s      p(90)=0s      p(95)=0s     
     http_req_waiting...............: avg=11.56ms min=2.54ms  med=10.77ms max=93.1ms  p(90)=15.31ms p(95)=17.13ms
     http_reqs......................: 63950  2130.804494/s
     iteration_duration.............: avg=11.71ms min=2.74ms  med=10.92ms max=93.92ms p(90)=15.47ms p(95)=17.31ms
     iterations.....................: 63950  2130.804494/s
     vus............................: 25     min=25        max=25 
     vus_max........................: 25     min=25        max=25 
```