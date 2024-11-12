### Установка OpenVPN на VPS с Ubuntu

1. Обновите пакеты:

```
sudo apt update  
sudo apt upgrade
```

2. Установите OpenVPN на сервер (server) Ubuntu:
```
sudo apt install openvpn easy-rsa net-tools
```

### Создание удостоверяющего центра

1. Скопируйте файлы OpenVPN в директорию /etc/openvpn/:
```
sudo cp -R /usr/share/easy-rsa /etc/openvpn/
```
Затем перейдите в папку с копиями файлов:
```
cd /etc/openvpn/easy-rsa
```

2. Сгенерируйте удостоверяющий центр и его корневой сертификат. Для этого поочередно выполните следующие команды:
```
export EASYRSA=$(pwd)  
sudo ./easyrsa init-pki  
sudo ./easyrsa build-ca
```

Далее введите пароль: он будет использоваться для подписи ключей и сертификатов в будущем.  
После этого будут созданы два файла:  
**/etc/openvpn/easy-rsa/pki/ca.crt** — сертификат, 
**/etc/openvpn/easy-rsa/pki/private/ca.key** — приватный ключ.

3. Создайте отдельную директорию для файлов сертификата:
```
mkdir /etc/openvpn/certs/
```
4. Переместите файл сертификата в созданную директорию:
```
cp /etc/openvpn/easy-rsa/pki/ca.crt  
/etc/openvpn/certs/ca.crt
```

На следующих этапах будет описана настройка OpenVPN на VPS.

### Создание серверных ключей

1. Создайте закрытый ключ и файл запроса. Для этого выполните команду:
```
./easyrsa gen-req server nopass
```

2. Подпишите файлы с помощью ключа, созданного на предыдущем этапе:
```
./easyrsa sign-req server server
```

После этого подтвердите действие и введите пароль, который вы сгенерировали ранее.

3. Скопируйте файл запроса и приватный ключ. Для этого поочередно выполните команды:
```
cp /etc/openvpn/easy-rsa/pki/issued/server.crt  
/etc/openvpn/certs/  
cp /etc/openvpn/easy-rsa/pki/private/server.key  
/etc/openvpn/certs/
```
4. Сформируйте файл параметров по стандарту Diffie–Hellman:
```
openssl dhparam -out /etc/openvpn/certs/dh2048.pem 2048
```

5. Сгенерируйте HMAC-ключ:
```
openvpn --genkey --secret /etc/openvpn/certs/ta.key
```

После выполненных действий в папке /etc/openvpn/certs/ должно быть пять файлов. Проверьте их наличие с помощью команды:

```
ls -l /etc/openvpn/certs/
```

Вывод будет иметь следующий вид:

```
total 24  
-rw------- 1 root root 1204 Jan 01 00:00 ca.crt  
-rw-r--r-- 1 root root 424 Jan 01 00:05 dh2048. pem  
-rw------- 1 root root 4608 Jan 01 00:10 server.crt  
-rw------- 1 root root 1704 Jan 01 00:15 server.key  
-rw------- 1 root root 636 Jan 01 00:20 ta.key
```

### Создание ключей клиента

1. Сгенерируйте закрытый ключ и запрос на сертификат:
```
./easyrsa gen-req client-1 nopass
```
2. Подпишите файл:
```
./easyrsa sign-req client client-1
```
После этого подтвердите действие и введите пароль, который вы сгенерировали ранее.

### Настройка конфига сервера OpenVPN на Ubuntu

Конфигурация сервера (server) OpenVPN зависит от настроек, которые прописаны в файле server.conf. В качестве примера мы задействовали стандартные значения.

Для настройки:

1. Откройте конфигурационный файл OpenVPN:
```
sudo nano /etc/openvpn/server.conf
```
2. Добавьте следующее содержимое:

```
port 1194 # Порт для OpenVPN

;proto tcp  # Протокол, который использует OpenVPN
proto udp

;dev tap  # Интерфейс
dev tun

ca /etc/openvpn/certs/ca.crt    # Сертификат CA
cert /etc/openvpn/certs/server.crt   # Приватный ключ сервера
key /etc/openvpn/certs/server.key   # не распространяется и хранится в секрете

dh /etc/openvpn/certs/dh2048.pem   # параметры Diffie Hellman

server 10.8.0.0 255.255.255.0  # IP и маска подсети

ifconfig-pool-persist /etc/openvpn/ipp.txt   # После перезапуска сервера клиенту будет выдан прежний IP

push "redirect–gateway def1 bypass–dhcp"    # Установка шлюза по умолчанию

keepalive 10 120   # Пинговать удаленный узел с интервалом в 10 секунд  # Если узел не отвечает в течение 120 секунд, то будет выполнена попытка повторного подключения к клиенту

remote-cert-tls client     # Защита от DoS–атак портов UDP с помощью HMAC
tls-auth /etc/openvpn/certs/ta.key 0    # файл хранится в секрете

cipher AES-256-CBC  # для клиентов нужно указывать такой же криптографический шифр

persist-key  # При падении туннеля не выключать интерфейсы, не перечитывать ключи
persist-tun

status openvpn–status.log  # Лог текущих соединений. Каждую минуту обрезается и перезаписываться.

verb 4 # Уровень вербальности. # 4 подходит для обычного использования # 5 и 6 помогают в отладке при решении проблем с подключением

explicit-exit-notify 1  # Предупреждение клиента о перезапуске сервера
```

**Важно:** если подсеть адресов 10.8.0.0/24 занята другими сервисами, укажите другой IP формата 10.x.x.x.  
  
Далее сохраните изменения с помощью комбинации клавиш **Ctrl + O** и закройте файл сочетанием **Ctrl + X.**

**Пример конфигурации VPS TimeWeb**

> [!/etc/openvpn/server.conf]
> port 1194
> ;proto tcp
> proto udp
> ;dev tap
> dev tun
> ca /etc/openvpn/certs/ca.crt
> cert /etc/openvpn/certs/server.crt
> key /etc/openvpn/certs/server.key
> dh /etc/openvpn/certs/dh2048.pem
> server 10.8.0.0 255.255.255.0
> ifconfig-pool-persist /etc/openvpn/ipp.txt
> push "redirect-gateway def1 bypass-dhcp"
> keepalive 10 120
> remote-cert-tls client
> tls-auth /etc/openvpn/certs/ta.key 0
> cipher AES-256-CBC
> persist-key
> persist-tun
> status openvpn–status.log
> verb 4
> explicit-exit-notify 1


3. Проверьте конфигурационный файл на ошибки:
```
openvpn /etc/openvpn/server.conf
```

Если конфиг настроен корректно, в последней строке вывода вы увидите сообщение **Initialization Sequence Completed.**

4. Запустите службу OpenVPN:
```
systemctl start openvpn@server.service
```

5. Добавьте OpenVPN в автозагрузку:
```
systemctl enable openvpn@server.service
```
6. Проверьте работу службы:
```
systemctl status openvpn@server.service
```

Если служба работает, в выводе отобразится статус **Active (running).**

### Включение маршрутизации трафика

1. Создайте новую директорию:
```
mkdir /root/bin/
```

2. Откройте файл скрипта маршрутизации:
```
sudo nano /root/bin/vpn_route.sh
```

3. Добавьте строки:
```
#!/bin/sh  
DEV='eth0' # Сетевой интерфейс для выхода в интернет  
```

  
```
PRIVATE=10.8.0.0/24  # Значение подсети
  
if [ -z "$DEV" ]; then  
DEV="$(ip route | grep default | head -n 1 | awk '{print $5}')"  
fi  
 
sysctl net.ipv4.ip_forward=1  # Маршрутизация транзитных IP-пакетов 

iptables -I FORWARD -j ACCEPT  # Проверка блокировки перенаправленного трафика iptables 
  
Преобразование адресов (NAT)  
iptables -t nat -I POSTROUTING -s $PRIVATE -o $DEV -j MASQUERADE  # Преобразование адресов (NAT)  
```

Где:

- **10.8.0.0/24** — ваша подсеть,
- **eth0** — название сетевого интерфейса. Его можно узнать при помощи команды:

```
route | grep '^default' | grep -o '[^ ]*$'
```
Далее сохраните изменения с помощью комбинации клавиш **Ctrl + O** и закройте файл сочетанием **Ctrl + X.**

4. Назначьте права измененному файлу:
```
chmod 755 /root/bin/vpn_route.sh
```

5. Запустите скрипт в тестовом режиме:

```
bash /root/bin/vpn_route.sh
```

6. Если скрипт завершился без ошибок, создайте файл:
```
sudo nano /etc/systemd/system/openvpn-server-routing.service
```

7. Добавьте строки:
```
[Unit]  
Description=Start server OpenVPN.  
[Service]  
ExecStart=/root/bin/vpn_route.sh  
[Install]  
WantedBy=multi-user.target
```
Далее сохраните изменения с помощью комбинации клавиш **Ctrl + O** и закройте файл сочетанием **Ctrl + X.**
  
8. Добавьте созданную службу в автозагрузку:
```
systemctl enable openvpn-server-routing
```
