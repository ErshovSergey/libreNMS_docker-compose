
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


## Добавляем в мониторинг Debian
Устанавливаем  
```
apt-get install snmpd snmp libsnmp-dev
```
Обеспечиваем запуск при загрузке  
```
systemctl enable snmpd
systemctl start snmpd
```
Копируем файл конфигурации
```
cp /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.original
```
Редактируем файл конфигурации на предмет на каком интерфейсе слушает snmpd - по умолчанию только на localhost  
```
#  Listen for connections from the local system only
agentAddress  udp:127.0.0.1:161,udp:192.168.43.188:161
```
### Настраиваем smnp version 3, для этого необходимо  
создать пользователя для аутентификации, установить пароль для него, установить пароль для шифрования, определить доступы, определить алгоритм шифрования
Одной командой (до запуска команды необходимо остановить snmpd - systemctl stop snmpd)
```
net-snmp-create-v3-user -ro -A SecUREDpass -a SHA -X StRongPASS -x AES snmpreadonly
```
Проверить работу можно командой
```
snmpwalk -v3 -a SHA -A SecUREDpass -x AES -X StRongPASS -l authPriv -u snmpreadonly localhost | head -10
```


net-snmp-create-v3-user -ro -A hsiyw73HJsdnm -a SHA -X jwken12nKjdwnmk2j3SS -x AES snmpreadonly


### Ссылки
по мотивам https://github.com/jarischaefer/docker-librenms
