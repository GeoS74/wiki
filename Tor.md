# Tor


### [[Установка Tor]]
### [[Подключение мостов Tor]]

### [[Решение для Debian]]

### [[Варианты для Ubuntu]]

### Почитать
#### Toriptables
[toriptables](https://hackware.ru/?p=3138)
У программы всего две значимых опции: **-l** и **-f**
```
-h, --help   показать справку и выйти
-l, --load   Эта опция загрузит правила tor iptables
-f, --flush  Эта опция сбросит правила iptables за стандартные
```

```
git clone https: //github.com/ruped24/toriptables2
cd toriptables2/
sudo mv toriptables2.py /usr/local/bin/
cd
toriptables2.py -h
```




[пакеты Tor](https://ubuntu.pkgs.org/20.04/ubuntu-universe-arm64/tor_0.4.2.7-1_arm64.deb.html)

[исходники Tor](https://www.torproject.org/download/tor/)
[что делать с исходниками](https://community.torproject.org/onion-services/setup/install/)

[Установка репозитория](https://support.torproject.org/apt/tor-deb-repo/)

[Установка репозитория Tor](https://support-torproject-org.translate.goog/relay-operators/operators-4/?_x_tr_sl=auto&_x_tr_tl=ru&_x_tr_hl=ru)

[Tor - рецепты, подсказки](https://hackware.ru/?p=10530)






### Не работает
>Для настройки используются два файла  torcc и torsocks.conf
>https://линуксблог.рф/napravlyaem-ves-trafik-cherez-tor-v-linux/


[новое решение](https://forum.ubuntu.ru/index.php?topic=314401.0)

1. [Копать сюда](https://www.linuxuprising.com/2018/10/how-to-install-and-use-tor-as-proxy-in.html)

Получить свой IP адрес с помощью [[wget]]:

```
$ wget -qO - https://api.ipify.org
```

[Как «торифицировать» вашу оболочку](https://portal.imprezahost.com/knowledgebase/664/Install-Tor-proxy-on-Ubuntu-20.04-Linux.html?language=dutch)

## [[Установка и удаление Tor-browser]]

[настройка Tor](https://wiki.archlinux.org/title/Tor_(%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9))

[Установка и настройка Tor в Ubuntu](https://help.ubuntu.ru/wiki/tor)

[Настройка мостов](https://zalinux.ru/?p=6049)


[Настройка Proxy](https://hackware.ru/?p=10201)

[флудилка на форуме](https://www.cyberforum.ru/ubuntu-linux/thread1587860.html)

[Читай вот это](https://stepsboard.com/ru/%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%82%D0%B5-%D0%BF%D1%80%D0%BE%D0%BA%D1%81%D0%B8-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80-tor-%D1%81-raspberry-pi-%D0%B4%D0%BB%D1%8F-%D1%83%D0%BF%D1%80%D0%B0)

_Приоритетно:_ [Использование torsocks](https://linuxconfig.org/install-tor-proxy-on-ubuntu-20-04-linux)

[Устранение проблем с подключением к Tor](https://support.torproject.org/ru/connecting/connecting-2/)

>Погуглить про iptables и netfilter

- [iptables для чайников](https://losst.ru/nastrojka-iptables-dlya-chajnikov)
- [iptables how to](https://help.ubuntu.com/community/IptablesHowTo)
- [обнулить firewall](https://www.digitalocean.com/community/tutorials/how-to-list-and-delete-iptables-firewall-rules-ru)

Посмотреть состояние:
```
$ tor
```


Вариант решения проблемы подключения (в файле torrc):
```
SocksPort 9150 
ControlPort 9151
```


[Настройка Tor в Linux](https://www.newalive.net/146-nastroyka-tor-v-linux.html)
```
SocksPort 9050 
SocksPort 10.10.1.25:9100 
SocksPolicy accept 10.10.1.0/24 
SocksPolicy reject * 
Log notice file /var/log/tor/notices.log 
RunAsDaemon 1 
DataDirectory /var/lib/tor 
ORPort 9001 
Address IP or HOSTNAME 
Nickname RelayName 
RelayBandwidthRate 1000 KB 
RelayBandwidthBurst 2000 KB 
AccountingMax 100 GB 
AccountingStart month 1 00:00 
ContactInfo FirstName LastName <MyName AT MailServer dot com> 
ExitPolicy reject *:* 

#BridgeRelay 1 
#PublishServerDescriptor 0
```


Ещё вариант [настройки Tor в Linux](https://www.kobzarev.com/soft/tor/):
```
SOKSPort 9050 CacheDNS UseDNSCache

SOCKSPolicy accept private:*,reject *:*
DataDirectory /var/lib/tor
Log notice file /var/log/tor/notice.log
ExcludeExitNodes {ru},{ua},{by},{kz},{??}
StrictNodes 1
```

[Ещё вариант настройки](https://eyakubovskiy.ru/2021/09/06/nastroyka-servisa-tor-v-ubuntu-20-04/)
```
DataDirectory /var/lib/tor  
PidFile /run/tor/tor.pid  
RunAsDaemon 1  
User debian-tor  
ControlSocket /run/tor/control GroupWritable RelaxDirModeCheck  
ControlSocketsGroupWritable 1  
SocksPort unix:/run/tor/socks WorldWritable  
CookieAuthentication 1  
CookieAuthFileGroupReadable 1  
CookieAuthFile /run/tor/control.authcookie  
Log notice syslog  
#AvoidDiskWrites 1  
SocksPort 127.0.0.1:9150
```

Перезапустить Tor с новыми настройками:
```
$ sudo systemctl enable tor.service  
$ sudo service tor restart
```

Или попробовать так:
```
$ sudo /etc/init.d/tor restart
$ sudo /etc/init.d/privoxy restart
```



#### Ещё варианты
[install TorBrowser](https://torrbrowser.ru/tor-browser-for-linux)
[delete TorBrowser](https://torrbrowser.ru/faq/how-to-uninstall-tor-browser-from-pc)


Default-ные настройки правил в iptables:
```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy DROP)
target     prot opt source               destination         
DOCKER-USER  all  --  anywhere             anywhere            
DOCKER-ISOLATION-STAGE-1  all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
DOCKER     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         

Chain DOCKER (1 references)
target     prot opt source               destination         

Chain DOCKER-ISOLATION-STAGE-1 (1 references)
target     prot opt source               destination         
DOCKER-ISOLATION-STAGE-2  all  --  anywhere             anywhere            
RETURN     all  --  anywhere             anywhere            
```

#tor