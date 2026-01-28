
# Закрыть доступ для root по SSH и сменить порт


Посмотреть всех пользователей:

```bash
cat /etc/passwd
```


##### Шаг 1 - добить нового пользователя

```bash
adduser geos
```

> Прим.: система попросит придумать пароль для пользователя, не забудь его!!!

Для изменения пароля надо выполнить:

```bash
sudo passwd <имя пользователя>
```

##### Шаг 2 - дать права root

```bash
usermod -aG sudo geos
```

##### Шаг 3 - скопировать SSH-ключ от root для нового пользователя

```bash
mkdir -p /home/geos/.ssh
cp ~/.ssh/authorized_keys /home/geos/.ssh/
chown -R geos:geos /home/geos/.ssh
chmod 700 /home/geos/.ssh
chmod 600 /home/geos/.ssh/authorized_keys
```


Теперь новый пользователь может заходить по тому же ключу, что и `root`.


##### Шаг 4 - изменить порт SSH и запрет доступа root

Отредактировать файл `/etc/ssh/sshd_config`

```bash
nano /etc/ssh/sshd_config
```

- Строка `Port 22` заменить на `Port 2222` 
- Для запрета подключения под `root`  пользователем поменять строку `PermitRootLogin yes` на `PermitRootLogin no`

Перезапустить SSH

```bash
systemctl restart sshd
```

> Перед завершением сеанса `root` обязательно проверить возможность подключения под новым пользователем и доступность `sudo` для него.


##### Дополнительно - отключение парольной аутентификации

1) Отредактировать файл `/etc/ssh/sshd_config` строку `PasswordAuthentication yes` заменить на `PasswordAuthentication no`
2) Установить  `PubkeyAuthentication yes`
 
> Если `/etc/ssh/sshd_config` подключает другие конфиги, то в них также установить `PasswordAuthentication no`

Перезапустить SSH

```bash
systemctl restart sshd
```



#ssh #безопасность