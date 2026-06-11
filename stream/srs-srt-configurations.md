# SRS SRT Конфигурации

1) Запускаем docker в Git Bash

**docker run**
```bash
docker run --rm -it -p 1935:1935 -p 1985:1985 -p 8080:8080 -p 10080:10080/udp -v "D:\AndroidProject\docs\stream\srs.conf":/usr/local/srs/conf/srs.conf ossrs/srs:v6.0-r0 ./objs/srs -c conf/srs.conf
```

проверяем что все работает http://localhost:8080/ или http://твой_ip:8080/

## Порты

| Порт  | Протокол | Транспорт | Назначение                       |
| ----- | -------- | --------- | -------------------------------- |
| 1935  | RTMP     | TCP       | Отправка и приём стрима          |
| 1985  | HTTP API | TCP       | API SRS (локально, наружу не надо) |
| 8080  | HTTP     | TCP       | Веб-консоль SRS                  |
| 10080 | SRT      | UDP       | Отправка и приём стрима          |

2.1) Отдаем картинку через OBS (по **RTMP**)
![[obs_send.png]]
2.2) Отдаем картинку через OBS (по **SRT**)
![[send_obs_srt.png]]
3) Смотрим в OBS (по RTMP)
   rtmp://localhost:1935/live/livestream
![[obs_receive.png|500]]

3.2) Смотреть в OBS (по SRT)

srt://IP:10080?streamid=#!::r=live/livestream,m=request

# Примеры стриминга

## Переменные
| Переменная           | Описание                 | Как узнать                                                 |
| -------------------- | ------------------------ | ---------------------------------------------------------- |
| `{your_server_ip}`   | IP адрес сервера с SRS   | Win: `ipconfig`, Mac/Linux: `ifconfig`, Local: `127.0.0.1` |
| `{name}`             | Имя стрима (любое)       | Придумай сам: `stream1`, `cam1`, `phone`                   |
| `10080`              | Порт SRT (UDP)           | Стандартный порт для SRT в конфигах выше                   |

## OBS настройка:
### Транслируем на сервер
- Сервер: `srt://93.84.96.193:10080`
- Stream Key: `#!::r=live/{name},m=publish`
### Считываем
- Источник медиа -> Ввод: `srt://93.84.96.193:10080?streamid=#!::r=live/{name},m=request`
- Источник медиа -> Ввод: `srt://93.84.96.193:10080?streamid=live/{name}`
- Источник медиа -> Ввод: `srt://93.84.96.193:10080?streamid=live/livestream`
- Источник медиа -> Ввод: `rtmp://127.0.0.1/live/livestream`

Включите «Использовать аппаратное декодирование»

## Camera настройка:
- `srt://{your_server_ip}:10080`

## Larix Настройка:
- url: `srt://93.84.96.193:10080`
- streamid:  `#!::r=live/{name},m=publish`


Попробовать отправлять видео с камеры в 3.000 kbps

| Качество | H.264     | H.265     |
| -------- | --------- | --------- |
| 1080p/60 | 6000 kbps | 4000 kbps |
| 1080p/30 | 4500 kbps | 3000 kbps |
| 720p/60  | 4500 kbps | 3000 kbps |


