# Сайт для использования библиотеки GreenTensor.

На текущий момент сайт лежит на Яндекс Клауде, в обычной виртуальной машине

## Деплой

### Настройка сертификата
Нам нужно получить сертификат (для HTTPS)

Сначала создаём директорию для Certbot
```bash
mkdir -p certbot/conf certbot/www
```

Запускаем Nginx для верификации
```bash
docker run -it --rm \
  -p 80:80 \
  -v $(pwd)/certbot/conf:/etc/letsencrypt \
  -v $(pwd)/certbot/www:/var/www/certbot \
  certbot/certbot certonly \
  --standalone \
  --agree-tos \
  --no-eff-email \
  -d greentensor.ru -d www.greentensor.ru \
```

Если мы очень ответственные, то делаем автообновление сертификата через **Cron**:

```bash
0 12 * * * docker-compose exec certbot certbot renew --quiet
```

### Запуск 

```bash
docker-compose up --build
```

### Возможные проблемы

Если вы вносите какие-то изменения в файлы сервера (у фронта, или у бэка, не суть важна), то можно словить ошибку поиска конфигурации образа (скорее всего проблема кэша). Тогда следует очистить Docker

```bash
docker system prune -a --volumes
docker network prune
```

И провести процедуру запуска заново