# Cвой VPN-сервер

[Источник](https://www.reg.ru/blog/sozdayem-sobstvennyy-vpn-server/)

## Что такое OpenVPN

**OpenVPN** — популярная реализация технологии VPN с открытым исходным кодом. С его помощью вы можете создать собственный VPN-сервер с высоким уровнем безопасности гибкими настройками. OpenVPN поддерживает Windows, MacOS, Linux, Android, iOS и ChromeOS, поэтому вы сможете использовать его на разных устройствах.

## Создаем собственный VPN-сервер: установка OpenVPN

Мы производили установку на сервер с ОС Ubuntu 20.04 — для других версий процесс установки и настройки может отличаться. Чтобы начать установку, [подключитесь к серверу по SSH](https://help.reg.ru/support/klassicheskie-vps/osnovi-raboty-s-vps/podklyucheniye-i-nastroyka-ssh-na-vps).

### Установка пакетов

1. Обновите списки пакетов с помощью команды:

```bash
sudo apt update
```

2. Установите пакеты, необходимые для установки и настройки OpenVPN:

```bash
sudo apt install openvpn easy-rsa net-tools
```

### Настройка удостоверяющего центра

После установки пакетов необходимо развернуть на сервере центр сертификации и сгенерировать корневой сертификат. Для этого: 

1. Скопируйте необходимые файлы в папку /etc/openvpn/, а затем перейдите в нее:

```bash
sudo cp -R /usr/share/easy-rsa /etc/openvpn/
cd /etc/openvpn/easy-rsa
```

2. Создайте удостоверяющий центр и корневой сертификат:

```bash
export EASYRSA=$(pwd)  
sudo ./easyrsa init-pki  
sudo ./easyrsa build-ca
```


После ввода последней команды вам будет предложено ввести кодовую фразу. Указанный вами пароль в дальнейшем будет использоваться для подписи сертификатов и ключей.

У вас появятся 2 файла:

- /etc/openvpn/easy-rsa/pki/ca.crt — сертификат CA
- /etc/openvpn/easy-rsa/pki/private/ca.key — приватный ключ CA. 

1. Создайте директорию, в которой будут храниться ключи и сертификаты:

```bash
mkdir /etc/openvpn/certs/
```

2. Переместите в созданную папку сертификат CA:

```bash
cp /etc/openvpn/easy-rsa/pki/ca.crt /etc/openvpn/certs/ca.crt
```


### Создание запроса сертификата и закрытого ключа сервера OpenVPN

1. Создайте закрытый ключ для сервера и файл запроса сертификата с помощью команды:

```bash
./easyrsa gen-req server nopass
```

2. Подпишите созданный сертификат ключом сертификационного центра:

```bash
./easyrsa sign-req server server
```

Затем введите «yes» и нажмите **Enter**. После введите кодовую фразу, которую вы задали при настройке удостоверяющего центра.

3. Скопируйте сгенерированный сертификат и ключ в директорию /etc/openvpn/certs/:

```bash
cp /etc/openvpn/easy-rsa/pki/issued/server.crt /etc/openvpn/certs/
cp /etc/openvpn/easy-rsa/pki/private/server.key /etc/openvpn/certs/
```

4. Сгенерируйте файл параметров Diffie–Hellman:

```bash
openssl dhparam -out /etc/openvpn/certs/dh2048.pem 2048
```

5. Создайте ключ HMAC с помощью команды:

```bash
openvpn --genkey --secret /etc/openvpn/certs/ta.key
```

Теперь в директории /etc/openvpn/certs/ должно быть 5 файлов. Проверить это можно с помощью команды:

```bash
ls -l /etc/openvpn/certs/
```

Вывод должен выглядеть следующим образом:

![](https://img.reg.ru/news/blog_25082023_image2.png)

### Создание ключей клиентов OpenVPN

1. Сгенерируйте закрытый ключ и запрос сертификата клиента:

```bash
./easyrsa gen-req client1 nopass
```

2. Затем подпишите запрос с помощью команды:

```bash
./easyrsa sign-req client client1
```

Введите «yes» и нажмите **Enter**. Затем введите кодовую фразу, которую вы задали при настройке удостоверяющего центра. 

>Прим.: Для каждого отдельного клиента надо создавать закрытый ключ.

## Запуск сервера OpenVPN

### Создание конфигурационного файла OpenVPN

Теперь приступим к созданию конфигурационного файла. Для этого:

1. Введите команду:

```bash
nano /etc/openvpn/server.conf
```


2. Скопируйте и вставьте в файл конфигурацию:

```
# Порт для OpenVPN
port 1194

# Протокол, который использует OpenVPN
;proto tcp
proto udp

# Интерфейс
;dev tap
dev tun

# Ключи

# Сертификат CA
ca /etc/openvpn/certs/ca.crt
# Сертификат сервера
cert /etc/openvpn/certs/server.crt
# Приватный ключ сервера
key /etc/openvpn/certs/server.key #не распространяется и хранится в секрете

# параметры Diffie Hellman
dh /etc/openvpn/certs/dh2048.pem

# Создание виртуальной сети и ее параметры

# IP и маска подсети
server 10.8.0.0 255.255.255.0

# После перезапуска сервера клиенту будет выдан прежний IP
ifconfig-pool-persist /etc/openvpn/ipp.txt

# Установка шлюза по умолчанию
push "redirect–gateway def1 bypass–dhcp"

# Разрешить использовать нескольким клиентами одну и ту же пару ключей
# не рекомендуется использовать, закомментирована
;duplicate–cn

# Пинговать удаленный узел с интервалом в 10 секунд
# Если узел не отвечает в течение 120 секунд, то будет выполнена попытка повторного подключения к клиенту
keepalive 10 120

# Защита от DoS–атак портов UDP с помощью HMAC
remote-cert-tls client
tls-auth /etc/openvpn/certs/ta.key 0 # файл хранится в секрете

# Криптографические шифры
cipher AES-256-CBC #для клиентов нужно указывать такой же

# Сжатие и отправка настроек клиенту
;compress lz4–v2
;push "compress lz4–v2"

# Максимальное число одновременных подключений
;max–clients 100

# Понижение привилегий демона OpenVPN
# после запуска
# Не использовать для Windows
;user nobody
;group nobody

# При падении туннеля не выключать интерфейсы, не перечитывать ключи
persist-key
persist-tun

# Лог текущих соединений
# Каждую минуту обрезается и перезаписываться
status openvpn–status.log

# Логи syslog
# Используется только один. Раскомментировать необходимый
# перезаписывать файл журнала при каждом запуске OpenVPN
;log openvpn.log

# дополнять журнал
;log–append openvpn.log

# Уровень вербальности
#
# 0 тихий, кроме фатальных ошибок
# 4 подходит для обычного использования
# 5 и 6 помогают в отладке при решении проблем с подключением
# 9 крайне вербальный
verb 4

# Предупреждение клиента о перезапуске сервера
explicit-exit-notify 1
```

>**Обратите внимание!** Если подсеть 10.8.0.0/24 уже занята, то в параметре **server** нужно указать другой IP-адрес, например, 10.10.0.0.

Сохраните изменения и закройте файл, нажав **Ctrl+X, Y, Enter**.

### Проверка корректной работы сервера OpenVPN

1. Проверьте, не возникают ли ошибки при запуске конфигурационного файла, с помощью команды:

```bash
openvpn /etc/openvpn/server.conf
```

Если все работает, то вы увидите сообщение «Initialization Sequence Completed»:

![](https://img.reg.ru/news/blog_25082023_image3.png)

1. Если ошибок нет, запустите службу OpenVPN и добавьте ее в автозагрузку:

```bash
systemctl start openvpn@server.service
systemctl enable openvpn@server.service
```

2. Проверьте, работает ли служба, с помощью команды:

```bash
systemctl status openvpn@server.service
```


![](https://img.reg.ru/news/blog_25082023_image4.png)

### Включение маршрутизации трафика на OpenVPN-сервере

Теперь необходимо настроить маршрутизацию трафика для доступа к глобальной сети. Для этого:

1. Создайте директорию /root/bin/:

```bash
mkdir /root/bin/
```

2. Введите команду:

```bash
nano /root/bin/vpn_route.sh
```

3. Добавьте в файл конфигурацию:

```
#!/bin/sh

# Сетевой интерфейс для выхода в интернет
DEV='eth0'

# Значение подсети
PRIVATE=10.8.0.0/24
if [ -z "$DEV" ]; then
DEV="$(ip route | grep default | head -n 1 | awk '{print $5}')"
fi

# Маршрутизация транзитных IP-пакетов
sysctl net.ipv4.ip_forward=1

# Проверка блокировки перенаправленного трафика iptables
iptables -I FORWARD -j ACCEPT

# Преобразование адресов (NAT)
iptables -t nat -I POSTROUTING -s $PRIVATE -o $DEV -j MASQUERADE
```

Если при создании конфигурационного файла OpenVPN вы задавали подсеть, отличную от 10.8.0.0/24, то укажите свое значение подсети в параметре **PRIVATE**.  
  
В параметре **DEV** нужно указать сетевой интерфейс, который используется для выхода в интернет. Узнать его можно с помощью команды:

```bash
route | grep '^default' | grep -o '[^ ]*$'
```

После добавления конфигурации сохраните изменения и закройте файл с помощью клавиш **Ctrl+X, Y, Enter**.

4. Задайте права для файла:

```bash
chmod 755 /root/bin/vpn_route.sh
```

5. Выполните тестовый запуск скрипта:

```bash
bash /root/bin/vpn_route.sh
```

6. Если ошибок нет, добавьте скрипт в автозагрузку. Для этого создайте файл:

```bash
nano /etc/systemd/system/openvpn-server-routing.service
```

7. Добавьте в него следующие данные:

```bash
[Unit]
Description=Включение маршрутизации OpenVPN трафика.
[Service]
ExecStart=/root/bin/vpn_route.sh
[Install]
WantedBy=multi-user.target
```

Чтобы сохранить файл и закрыть его, нажмите **Ctrl+X, Y, Enter.**

8. Добавьте созданную службу в автозагрузку:

```bash
systemctl enable openvpn-server-routing
```

## Настройка клиента OpenVPN

Теперь необходимо создать файл конфигурации .ovpn для подключения клиента к VPN. Для этого откройте любой текстовый редактор и добавьте в него конфигурацию:

```
# Роль
client

# IP сервера OpenVPN
remote 123.123.123.123

# Порт сервера OpenVPN, как в конфигурации сервера
port 1194

# Интерфейс
dev tun

# Протокол OpenVPN, как на сервере
;proto tcp
proto udp

# Имя хоста, IP и порт сервера
;remote my–server–1 1194
;remote my–server–2 1194

# Случайный выбор хостов. Если не указано, берется по порядку
;remote–random

# Преобразование имени хоста
# (в случае непостоянного подключения к интернету)
resolv-retry infinite

# Привязка к локальному порту
nobind

# Шлюз по умолчанию
redirect-gateway def1 bypass-dhcp

# При падении туннеля не выключать интерфейсы, не перечитывать ключи
persist-key
persist-tun

# Настройка HTTP прокси при подключении OpenVPN серверу
;http–proxy–retry # retry on connection failures
;http–proxy [proxy server] [proxy port #]

# Отключение предупреждений о дублировании пакетов
;mute–replay–warnings

# Дополнительная защита
remote-cert-tls server

# Ключ HMAC
key-direction 1

# Шифрование
cipher AES-256-CBC

# Сжатие. Если на сервере отключено, не включается
#comp–lzo
# Вербальность журнала
verb 3

# Сертификаты
<ca>
*Вставьте содержимое файла /etc/openvpn/certs/ca.crt*
</ca>
<cert>
*Вставьте содержимое файла /etc/openvpn/easy-rsa/pki/issued/client1.crt*
</cert>
<key>
*Вставьте содержимое файла /etc/openvpn/easy-rsa/pki/private/client1.key*
</key>
<tls-auth>
*Вставьте содержимое файла /etc/openvpn/certs/ta.key*
</tls-auth>
```

В параметре **remote** вместо 123.123.123.123 укажите IP-адрес вашего сервера. В тегах вставьте содержимое сгенерированных ранее файлов:

- **< ca>< /ca>** **<ca></ca>** — вставьте данные из файла /etc/openvpn/certs/ca.crt,
- **< cert>< /cert>** **<cert></cert>** — вставьте данные из файла /etc/openvpn/easy-rsa/pki/issued/client1.crt,
- **<key></key>** **< key>< /key>** — вставьте данные из файла /etc/openvpn/easy-rsa/pki/private/client1.key,
- **< tls-auth>< /tls-auth>** — вставьте данные из файла /etc/openvpn/certs/ta.key.

Вы можете вывести содержимое файла в терминал, затем скопировать его и вставить в конфигурацию. Чтобы вывести содержимое файла, воспользуйтесь командой:

```bash
cat путь_к_файлу
```

После внесения всех изменений сохраните конфигурационный файл с расширением **.ovpn**.

Чтобы подключиться к нашему VPN, необходимо скачать клиент OpenVPN для вашего устройства и импортировать в него созданный конфигурационный файл .ovpn.

>файл `.ovpn` не может быть сохранён под таким именем на iPhone, его надо переименовать, напр. в `client.ovpn`


>Прим.: Для каждого отдельного клиента надо создавать закрытый ключ т прописывать данные в `< cert>< /cert>` и `< key>< /key>` файла `.ovpn`

Теперь ваш собственный VPN-сервер запущен и полностью готов к использованию. Надеемся, что наша инструкция по созданию собственного VPN-сервера была для вас полезной и вам больше не придется беспокоиться о том, что злоумышленники перехватят ваши данные.

#openvpn