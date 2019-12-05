
## Установка  
### Копируем настройки по умолчанию  
```
cp .env-default .env
```
### Создаем ключ  
```
docker run --rm jarischaefer/docker-librenms generate_key
```
Помещаем в .env в APP_KEY включая "base64:*"  

### Собираем  
```
docker-compose up --build -d --remove-orphans --force-recreate
```
### Создаем БД  
```
docker exec nms.erchov.ru_nginx setup_database
```
### Создаем первого пользователя  
```
docker exec librenms create_admin
    User: admin
    Password: admin
```

