## 1. Создать docker-compose.yml

```bash
cat > docker-compose.yml << 'EOF'
version: '3'
services:
  srs:
    image: ossrs/srs:latest
    ports:
      - "1985:1985"      # HTTP API
      - "8080:8080"      # HTTP Server (веб-интерфейс)
      - "10080:10080/udp"  # SRT порт для камеры
    environment:
      - SRS_SRT_SERVER_ENABLED=on
      - SRS_SRT_SERVER_LISTEN=10080
      - SRS_SRT_SERVER_MAXBW=-1
      - SRS_SRT_SERVER_MSS=1500
      - SRS_SRT_SERVER_LATENCY=1000
      - SRS_SRT_SERVER_SENDBUF=33554432
      - SRS_SRT_SERVER_RECVBUF=33554432
      - SRS_SRT_SERVER_TLPKTDROP=on
      - SRS_SRT_SERVER_TSBPDMODE=on

  cloudflared:
    image: cloudflare/cloudflared:latest
    command: tunnel --no-autoupdate --url http://srs:8080
    depends_on:
      - srs
EOF
```

## 2. Запустить

```bash
docker-compose up -d
```

## 3. Получить публичный URL

```bash
docker-compose logs cloudflared | grep trycloudflare.com
```