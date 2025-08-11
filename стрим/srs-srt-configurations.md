# SRS SRT Конфигурации

## 1. Базовая SRT конфигурация

**Назначение:** Простой запуск SRS с поддержкой SRT для тестирования и базового использования. Подходит для начального знакомства с SRT протоколом.

```bash
docker run --rm -it \
  -p 8085:1985 \
  -p 8090:8080 \
  -p 8080:10080/udp \
  ossrs/srs:latest ./objs/srs -c conf/srt.conf
```

## 2. Ультра низкая задержка (100-300ms)

**Назначение:** Для интерактивных стримов, онлайн-игр, видеозвонков и других приложений где критична минимальная задержка. Жертвует качеством ради скорости.

```bash
docker run --rm -it \
  -p 8085:1985 \
  -p 8090:8080 \
  -p 8080:10080/udp \
  -e SRS_SRT_SERVER_ENABLED=on \
  -e SRS_SRT_SERVER_LISTEN=10080 \
  -e SRS_SRT_SERVER_LATENCY=200 \
  -e SRS_SRT_SERVER_RECVLATENCY=200 \
  -e SRS_SRT_SERVER_PEERLATENCY=200 \
  -e SRS_SRT_SERVER_TLPKTDROP=on \
  -e SRS_SRT_SERVER_TSBPDMODE=on \
  -e SRS_VHOST_MIN_LATENCY=on \
  ossrs/srs:latest
```

## 3. Стабильная сеть (300-500ms)

**Назначение:** Оптимальный баланс между качеством и задержкой для стабильных сетей. Подходит для большинства live-стримов на YouTube, Twitch и других платформах.

```bash
docker run --rm -it \
  -p 8085:1985 \
  -p 8090:8080 \
  -p 8080:10080/udp \
  -e SRS_SRT_SERVER_ENABLED=on \
  -e SRS_SRT_SERVER_LISTEN=10080 \
  -e SRS_SRT_SERVER_LATENCY=400 \
  -e SRS_SRT_SERVER_RECVLATENCY=400 \
  -e SRS_SRT_SERVER_PEERLATENCY=400 \
  -e SRS_SRT_SERVER_TLPKTDROP=on \
  -e SRS_SRT_SERVER_TSBPDMODE=on \
  -e SRS_SRT_SERVER_SENDBUF=2097152 \
  -e SRS_SRT_SERVER_RECVBUF=2097152 \
  ossrs/srs:latest
```

## 4. Высокое качество (нестабильная сеть)

**Назначение:** Максимальное качество видео для нестабильных сетей с потерями пакетов. Используется для профессионального вещания где качество важнее задержки.

```bash
docker run --rm -it \
  -p 8085:1985 \
  -p 8090:8080 \
  -p 8080:10080/udp \
  -e SRS_SRT_SERVER_ENABLED=on \
  -e SRS_SRT_SERVER_LISTEN=10080 \
  -e SRS_SRT_SERVER_LATENCY=3000 \
  -e SRS_SRT_SERVER_RECVLATENCY=3000 \
  -e SRS_SRT_SERVER_PEERLATENCY=3000 \
  -e SRS_SRT_SERVER_TLPKTDROP=off \
  -e SRS_SRT_SERVER_TSBPDMODE=off \
  -e SRS_SRT_SERVER_SENDBUF=16777216 \
  -e SRS_SRT_SERVER_RECVBUF=16777216 \
  -e SRS_SRT_SERVER_CONNECT_TIMEOUT=20000 \
  ossrs/srs:latest
```

## 5. Максимальная производительность

**Назначение:** Для высоконагруженных систем с множественными стримами. Оптимизирован для максимальной пропускной способности и больших буферов.

```bash
docker run --rm -it \
  -p 8085:1985 \
  -p 8090:8080 \
  -p 8080:10080/udp \
  -e SRS_SRT_SERVER_ENABLED=on \
  -e SRS_SRT_SERVER_LISTEN=10080 \
  -e SRS_SRT_SERVER_MAXBW=-1 \
  -e SRS_SRT_SERVER_MSS=1500 \
  -e SRS_SRT_SERVER_LATENCY=1000 \
  -e SRS_SRT_SERVER_SENDBUF=33554432 \
  -e SRS_SRT_SERVER_RECVBUF=33554432 \
  -e SRS_SRT_SERVER_TLPKTDROP=on \
  -e SRS_SRT_SERVER_TSBPDMODE=on \
  ossrs/srs:latest
```

## 6. Защищенный SRT с шифрованием

**Назначение:** Безопасная передача конфиденциального контента с AES шифрованием. Используется для корпоративных стримов и защищенных трансляций.

```bash
docker run --rm -it \
  -p 8085:1985 \
  -p 8090:8080 \
  -p 8080:10080/udp \
  -e SRS_SRT_SERVER_ENABLED=on \
  -e SRS_SRT_SERVER_LISTEN=10080 \
  -e SRS_SRT_SERVER_PASSPHRASE="your_secret_password_here" \
  -e SRS_SRT_SERVER_PBKEYLEN=32 \
  -e SRS_SRT_SERVER_LATENCY=1000 \
  -e SRS_SRT_SERVER_TLPKTDROP=on \
  ossrs/srs:latest
```

## Параметры SRT сервера

### Основные настройки
- **enabled**: Включить SRT сервер (on/off)
- **listen**: UDP порт для SRT (по умолчанию 10080)
- **maxbw**: Максимальная пропускная способность (-1=бесконечно, 0=авто, >0=байт/сек)
- **mss**: Максимальный размер сегмента (по умолчанию 1500 байт)

### Настройки задержки
- **latency**: Общая задержка (устанавливает recvlatency и peerlatency)
- **recvlatency**: Задержка приемника (мс)
- **peerlatency**: Задержка отправителя (мс)

### Буферы
- **sendbuf**: Размер буфера отправки (байты)
- **recvbuf**: Размер буфера получения (байты)

### Контроль качества
- **tlpktdrop**: Сбрасывать поздние пакеты (on/off)
- **tsbpdmode**: Доставка пакетов по временным меткам (on/off)

### Безопасность
- **passphrase**: Пароль шифрования (10-79 символов)
- **pbkeylen**: Длина ключа шифрования (0/16/24/32)

### Тайм-ауты
- **connect_timeout**: Тайм-аут соединения (мс)
- **peer_idle_timeout**: Тайм-аут простоя соединения (мс)


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
