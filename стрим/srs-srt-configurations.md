# SRS SRT Конфигурации

1) Запускаем docker в Git Bash

**Вариант 1: docker-compose (рекомендуется)**
```bash
# Локалка/WiFi
docker-compose -f docker-compose.local.yml up

# 5G / хороший 4G
docker-compose -f docker-compose.speed.yml up

# Типичный 4G
docker-compose -f docker-compose.balance.yml up

# Плохая сеть
docker-compose -f docker-compose.quality.yml up
```

**Вариант 2: docker run**
```bash
docker run --rm -it \
  -p 1935:1935 \
  -p 8080:8080 \
  -p 10080:10080/udp \
  -v "D:\AndroidProject\docs\стрим\local.txt":/usr/local/srs/conf/srt.conf \
  ossrs/srs:v6.0-r0 \
  ./objs/srs -c conf/srt.conf
```

проверяем что все работает http://localhost:8080/ или http://твой_ip:8080/

2.1) Траслируем в OBS (по **RTMP**)
![[obs_send.png]]
2.2) Траслируем в OBS (по **SRT**)
![[send_obs_srt.png]]
3) Смотрим в OBS (по RTMP)
   rtmp://localhost:1935/live/livestream
![[obs_receive.png|500]]

3.2) Смотреть в OBS (по SRT)

srt://IP:10080?streamid=#!::r=live/livestream,m=request

| Конфиг   | Latency   | Буферы | tlpktdrop | tsbpdmode | Сценарий          | RTT       |
| -------- | --------- | ------ | --------- | --------- | ----------------- | --------- |
| LOCAL    | 120ms     | 2MB    | ON        | ON        | Локалка/WiFi      | 1-30ms    |
| SPEED    | 400ms     | 1.5MB  | ON        | ON        | 5G/хороший 4G     | ~100ms    |
| BALANCED | 600ms     | 3MB    | ON        | ON        | Типичный 4G       | ~150ms    |
| QUALITY  | 0 (адапт) | 10MB   | OFF       | OFF       | Плохая сеть       | любой     |


# Примеры стриминга

## Переменные
| Переменная           | Описание                 | Как узнать                                                 |
| -------------------- | ------------------------ | ---------------------------------------------------------- |
| `{your_server_ip}`   | IP адрес сервера с SRS   | Win: `ipconfig`, Mac/Linux: `ifconfig`, Local: `127.0.0.1` |
| `{name}`             | Имя стрима (любое)       | Придумай сам: `stream1`, `cam1`, `phone`                   |
| `10080`              | Порт SRT (UDP)           | Стандартный порт для SRT в конфигах выше                   |

## OBS настройка:
### Транслируем на сервер
- Сервер: `srt://{your_server_ip}:10080`
- Stream Key: `#!::r=live/{name},m=publish`
### Считываем
- Источник медиа -> Вввод: `srt://{your_server_ip}:10080?streamid=#!::r=live/{name},m=publish`
- Источник медиа -> Вввод: `srt://{your_server_ip}:10080?streamid=live/{name}`

## Camera настройка:
- `srt://{your_server_ip}:10080`

## Larix Настройка:
- url: `srt://{your_server_ip}:10080`
- streamid:  `#!::r=live/{name},m=publish`


Попробовать отправлять видео с камеры в 3.000 kbps

| Качество | H.264     | H.265     |
| -------- | --------- | --------- |
| 1080p/60 | 6000 kbps | 4000 kbps |
| 1080p/30 | 4500 kbps | 3000 kbps |
| 720p/60  | 4500 kbps | 3000 kbps |
