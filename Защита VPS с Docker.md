# Защита VPS с Docker

## **Какие угрозы возможны?**

1. **Сканирование портов**
    - Боты проверяют открытые порты (22/SSH, 2375/Docker API и др.), даже если у вас нет веб-сервера.
    - Пример: запросы к `80/tcp` или `443/tcp` (если порты открыты, но ничего не слушает).
        
2. **Атаки на Docker**
    
    - Если Docker API доступен извне (порт `2375`/`2376`), возможны:
        - Запуск вредоносных контейнеров.
        - Кража данных через `volume`-монтирования.
            
3. **Подбор SSH**
    - Если открыт `22/tcp`, брутфорс логинов (`root:password123`) — обычное дело.
        
4. **Ложные PHP-запросы (как в вашем случае)**
    - Боты шлют мусорные HTTP-запросы, но **без веб-сервера они просто "горят впустую"**.

## **Как защититься?**

### 1. **Закройте ненужные порты**
- Проверьте открытые порты:

```bash
ss -tulnp | grep LISTEN
```


- Если Docker API (`2375`/`2376`) не нужен извне — закройте его в `docker.service`:

```
# /etc/systemd/system/docker.service.d/override.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2376
```

Затем:

```bash
systemctl daemon-reload
systemctl restart docker
```

### 2. **Настройте SSH**

- Отключите вход под `root`:

```bash
sed -i 's/^PermitRootLogin .*/PermitRootLogin no/' /etc/ssh/sshd_config
```


- Включите аутентификацию по ключу:

```bash
sed -i 's/^#PasswordAuthentication .*/PasswordAuthentication no/' /etc/ssh/sshd_config
```

- Перезапустите SSH:

 ```bash
 systemctl restart sshd
``` 

### 3. **Firewall (UFW)**

- Разрешите только нужное (например, SSH на нестандартный порт):

```bash
ufw allow 2222/tcp  # Вместо 22
ufw enable
```


### 4. **Если веб-сервер не нужен**

- Убедитесь, что `80/tcp` и `443/tcp` закрыты:

```bash
sudo ufw deny 80/tcp
sudo ufw deny 443/tcp
```

