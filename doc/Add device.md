## Добавляем мониторинг устройства по snmp

### Добавляем в мониторинг Debian
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
Уточняем контактную информацию и местонахождение в строках   
```
sysLocation    Sitting on the Dock of the Bay
sysContact     Me <me@example.org>
```
При необхдимости уточняем остальные параметры.  
После изменений перезапускаем  
```
systemctl restart snmpd
```
### Настраиваем smnp version 3 для Debian, для этого необходимо  
Создать пользователя для аутентификации, установить пароль для него, установить пароль для шифрования, определить доступы, определить алгоритм шифрования  
Одной командой (до запуска команды необходимо остановить _snmpd - systemctl stop snmpd_)  
```
net-snmp-create-v3-user -ro -A SecUREDpass -a MDA -X StRongPASS -x DES snmp_read
```
Проверить работу можно командой (сначала запустив _systemctl start snmpd_)  
```
snmpwalk -v3 -a MD5 -A SecUREDpass -x DES -X StRongPASS -l authPriv -u snmp_read localhost | head -10
```
