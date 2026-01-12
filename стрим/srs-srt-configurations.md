# SRS SRT Конфигурации


После запуска можно открыть браузер http://127.0.0.1:8090/ и смотреть инфу

1) Запускаем docker в Git Bash

```bash
docker run --rm -it \
  -p 1935:1935 \
  -p 8080:8080 \
  -p 10080:10080/udp \
  -v "D:\srt\local.txt":/usr/local/srs/conf/srt.conf \
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

| Конфиг   | Latency   | Буферы | tlpktdrop | tsbpdmode | Сценарий     |
| -------- | --------- | ------ | --------- | --------- | ------------ |
| LOCAL    | 50ms      | 500KB  | ON        | ON        | Локалка      |
| QUALITY  | 0 (адапт) | 10MB   | OFF       | OFF       | Плохая сеть  |
| BALANCED | 120ms     | 3MB    | ON        | ON        | Норм сеть    |
| SPEED    | 300ms     | 1.5MB  | ON        | ON        | Хорошая сеть |


### Примеры стриминга

**OBS настройка:**
- Сервер: `srt://your_server_ip:8080`
- Stream Key: `#!::r=live/livestream,m=publish`

- Cервер: `srt://your_server_ip:8080:?streamid=#!::r=live/livestream,m=publish`

- Смотреть: `srt://your_server_ip:8080?streamid=live/livestream`

Camera настройка:
- `srt://127.0.0.1:8080`

Larix Настройка
- url: `srt://127.0.0.1:8080`
- streamid:  `#!::r=live/livestream1,m=publish`


Попробовать отправлять видео с камеры в 3.000 kbps

| Качество | H.264     | H.265     |
| -------- | --------- | --------- |
| 1080p/60 | 6000 kbps | 4000 kbps |
| 1080p/30 | 4500 kbps | 3000 kbps |
| 720p/60  | 4500 kbps | 3000 kbps |