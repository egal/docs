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

Результат запуска скрипта с k6 (2723 t/s)

```
running (0m30.0s), 00/25 VUs, 81719 complete and 0 interrupted iterations
default ✓ [======================================] 25 VUs  30s

     data_received..................: 40 MB  1.3 MB/s
     data_sent......................: 7.4 MB 248 kB/s
     http_req_blocked...............: avg=2.82µs  min=814ns  med=2.42µs  max=1.33ms  p(90)=2.88µs  p(95)=3.39µs 
     http_req_connecting............: avg=227ns   min=0s     med=0s      max=1.3ms   p(90)=0s      p(95)=0s     
     http_req_duration..............: avg=9.1ms   min=1.43ms med=8.12ms  max=8.11s   p(90)=12.35ms p(95)=14.08ms
       { expected_response:true }...: avg=9.1ms   min=1.43ms med=8.12ms  max=8.11s   p(90)=12.35ms p(95)=14.08ms
     http_req_failed................: 0.00%  ✓ 0           ✗ 81719
     http_req_receiving.............: avg=41.68µs min=13µs   med=38.46µs max=10.21ms p(90)=50.46µs p(95)=55.48µs
     http_req_sending...............: avg=12.47µs min=4.25µs med=11.23µs max=8.92ms  p(90)=14.7µs  p(95)=17µs   
     http_req_tls_handshaking.......: avg=0s      min=0s     med=0s      max=0s      p(90)=0s      p(95)=0s     
     http_req_waiting...............: avg=9.04ms  min=1.39ms med=8.07ms  max=8.11s   p(90)=12.29ms p(95)=14.02ms
     http_reqs......................: 81719  2723.080176/s
     iteration_duration.............: avg=9.16ms  min=1.48ms med=8.19ms  max=8.11s   p(90)=12.42ms p(95)=14.16ms
     iterations.....................: 81719  2723.080176/s
     vus............................: 23     min=23        max=25 
     vus_max........................: 25     min=25        max=25 
```