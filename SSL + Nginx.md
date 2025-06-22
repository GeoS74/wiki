# SSL + Nginx

## Этап 1

Получить образы `nginx:latest` и `certbot/certbot:latest`.

## Этап 2

Добавить образ `certbot/certbot` в файл `docker-compose.yml`.

```yml
  react:
    image: "geos74/signal:0.0.1"
    ports:
      - "80:80"
      - "443:443"
    restart: on-failure
    volumes:
      - ./default.conf:/etc/nginx/conf.d/default.conf
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    command: '/bin/sh -c ''while :; do sleep 6h & wait $${!}; nginx -s reload; done & nginx -g "daemon off;"'''

  certbot:
    image: certbot/certbot
    restart: unless-stopped #+++
    volumes:
      - ./certbot/conf:/etc/letsencrypt/:rw
      - ./certbot/www:/var/www/certbot/:rw
    entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"
```



## Этап 3


Изменить [[Конфигурация nginx | конфигурацию nginx]].  

Файл `default.conf`:

```
client_max_body_size 50m;

server {
  listen 80;

  location /.well-known/acme-challenge/ { root /var/www/certbot; }
  # Путь по которому certbot сможет проверить сервер на подлинность

  location / {
    root   /usr/share/nginx/html;
    try_files $uri /index.html;

  }

  location /api/bridge/ {
    proxy_pass http://bridge_app:3500;
  }
}
```

## Этап 4

Получить скрипт для установки сертификатов:

```bash
curl -L https://raw.githubusercontent.com/dancheskus/nginx-docker-ssl/master/init-letsencrypt.sh > init-letsencrypt.sh
```

Затем отредактировать полученный `init-letsencrypt.sh`:

- вместо `example.com` свои домены через пробел
- email
- меняем пути, если меняли их в папке
- `staging` временно ставим 1 (для теста) или 0 (если знаешь что делаешь)

ВАЖНО!!! надо изменить пути получения данных на эти (просто скопируй это):

```sh
curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot-nginx/certbot_nginx/_internal/tls_configs/options-ssl-nginx.conf > "$data_path/conf/options-ssl-nginx.conf"

curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot/certbot/ssl-dhparams.pem > "$data_path/conf/ssl-dhparams.pem"
```

ВАЖНО!!! перед запуском скрипта не забудь запустить контейнеры с [[Nginx]].

[[chmod|Расширить права ]] на этот файл скрипта  и запустить его:

```bash
chmod +x init-letsencrypt.sh

./init-letsencrypt.sh
```

>Прим: `docker-compose run --rm --entrypoint "certbot certificates" certbot` - позволяет проверить зарегистрированные сертификаты и узнать их срок годности.
## Этап 5

Изменить [[Конфигурация nginx | конфигурацию nginx]].  

Файл `default.conf`:

```
client_max_body_size 50m;

server {
  listen 80;
  listen 443 ssl;
  # Слушаем на портах 80 и 443

  server_name sgn74.ru www.sgn74.ru;
  # Этот сервер блок выполняется при этих доменных именах

  ssl_certificate /etc/letsencrypt/live/sgn74.ru/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/sgn74.ru/privkey.pem;
  # ssl_certificate и ssl_certificate_key - необходимые сертификаты

  include /etc/letsencrypt/options-ssl-nginx.conf;
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
  # include и ssl_dhparam - дополнительные, рекомендуемые Let's Encrypt, параметры

  # Определяем, нужен ли редирект с www на без www'шную версию
  if ($server_port = 80) { set $https_redirect 1; }
  if ($host ~ '^www\.') { set $https_redirect 1; }
  if ($https_redirect = 1) { return 301 https://sgn74.ru$request_uri; }

  location /.well-known/acme-challenge/ { root /var/www/certbot; }
  # Путь по которому certbot сможет проверить сервер на подлинность
  
  location / {
    root   /usr/share/nginx/html;
    try_files $uri /index.html;
  }

  location /api/bridge/ {
    proxy_pass http://bridge_app:3500;
  }
}
```

## Этап 6

Перезапускаем контейнеры. Всё должно работать.

## Итоговые файлы

##### docker-compose.yml

```yml
version: '1.0'

services:
  bridge_app:
    image: "geos74/bridge:0.0.3"
    environment:
      - SERVER_PORT=3500
      - DB_USER=bridge
      - DB_HOST=db_bridge
      - DB_NAME=bridge
      - DB_PASS=admin
      - NODE_ENV=dev
      - JWT_CHECK=true
      - JWT_SECRET_KEY=secret
      - ACTUAL_TTL=30 day
    volumes:
      - init-db-bridge:/bridge/libs
      - card-photo-bridge:/bridge/files/photo
    deploy:
      resources:
        limits:
          memory: 500M
    restart: on-failure
  db_bridge:
    image: "postgres"
    volumes:
      - init-db-bridge:/docker-entrypoint-initdb.d
      - db-bridge:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=secret_db_pass
      - POSTGRES_USER=db_user
      - POSTGRES_DB=bridge
    deploy:
      resources:
        limits:
          memory: 100M
    restart: on-failure

  react:
    image: "geos74/signal:0.0.1"
    ports:
      - "80:80"
      - "443:443"
    restart: on-failure
    volumes:
      - ./default.conf:/etc/nginx/conf.d/default.conf
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    command: '/bin/sh -c ''while :; do sleep 6h & wait $${!}; nginx -s reload; done & nginx -g "daemon off;"'''

  certbot:
    image: certbot/certbot
    restart: unless-stopped #+++
    volumes:
      - ./certbot/conf:/etc/letsencrypt/:rw
      - ./certbot/www:/var/www/certbot/:rw
    entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"

volumes:
  init-db-bridge:
  db-bridge:
  card-photo-bridge:
```

##### default.conf
```
client_max_body_size 50m;

server {
  listen 80;
  listen 443 ssl;
  # Слушаем на портах 80 и 443
  
  server_name sgn74.ru www.sgn74.ru;
  # Этот сервер блок выполняется при этих доменных именах

  ssl_certificate /etc/letsencrypt/live/sgn74.ru/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/sgn74.ru/privkey.pem;
  # ssl_certificate и ssl_certificate_key - необходимые сертификаты

  include /etc/letsencrypt/options-ssl-nginx.conf;
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
  # include и ssl_dhparam - дополнительные, рекомендуемые Let's Encrypt, параметры

  # Определяем, нужен ли редирект с www на без www'шную версию
  if ($server_port = 80) { set $https_redirect 1; }
  if ($host ~ '^www\.') { set $https_redirect 1; }
  if ($https_redirect = 1) { return 301 https://sgn74.ru$request_uri; }

  location /.well-known/acme-challenge/ { root /var/www/certbot; }
  # Путь по которому certbot сможет проверить сервер на подлинность

  location / {
    root   /usr/share/nginx/html;
    try_files $uri /index.html;
  }

  location /api/bridge/ {
    proxy_pass http://bridge_app:3500;
  }

}
```

##### init-letsencrypt.sh
```sh
#!/bin/bash

domains=("sgn74.ru www.sgn74.ru")

email="info@sgn74.ru" # Adding a valid address is strongly recommended

staging=0 # Set to 1 if you're testing your setup to avoid hitting request limits

data_path="./certbot"

rsa_key_size=4096

regex="([^www.].+)"


# root required
if [ "$EUID" -ne 0 ]; then echo "Please run $0 as root." && exit; fi

clear

# Menu for existing folder
for domain in ${domains[@]}; do
  domain_name=`echo $domain | grep -o -P $regex`
  if [ -d "$data_path/conf/live/$domain_name" ]; then
    echo "### Existing data found for some domains..."
    echo
    PS3='Your choice: '
    select opt in "Skip registered domains" "Remove registered domains and continue" "Remove registered domains and exit" "Exit"; do
      echo; echo;
      case $REPLY in
          1) echo " Installed certificates will be skipped" echo; echo; break;;
          2) echo " Old certificates removed"; echo; echo; rm -rf "$data_path"; break;;
          3) echo " Old certificates removed"; echo; rm -rf "$data_path"; echo " Exit..."; echo; echo; sleep 2; clear; exit;;
          4) echo " Exit..."; echo; echo; sleep 0.5; clear; exit;;
          *) echo "invalid option $REPLY";;
      esac
    done
    break
  fi
done

mkdir -p "$data_path"

if [ ! -e "$data_path/conf/options-ssl-nginx.conf" ] && [ ! -e "$data_path/conf/ssl-dhparams.pem" ]; then
  echo "### Downloading recommended TLS parameters ..."
  mkdir -p "$data_path/conf"

  curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot-nginx/certbot_nginx/_internal/tls_configs/options-ssl-nginx.conf > "$data_path/conf/options-ssl-nginx.conf"

  curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot/certbot/ssl-dhparams.pem > "$data_path/conf/ssl-dhparams.pem"
fi

# Dummy certificate
for domain in ${!domains[*]}; do
  domain_set=(${domains[$domain]})
  domain_name=`echo ${domain_set[0]} | grep -o -P $regex`
  mkdir -p "$data_path/conf/live/$domain_name"

  if [ ! -e "$data_path/conf/live/$domain_name/cert.pem" ]; then
    echo "### Creating dummy certificate for $domain_name domain..."
    path="/etc/letsencrypt/live/$domain_name"
    docker-compose run --rm --entrypoint "openssl req -x509 -nodes -newkey rsa:1024 \
    -days 1 -keyout '$path/privkey.pem' -out '$path/fullchain.pem' -subj '/CN=localhost'" certbot
  fi
done

echo "### Starting nginx ..."

# Restarting for case if nginx container is already started
docker-compose up -d nginx && docker-compose restart nginx

# Select appropriate email arg
case "$email" in
  "") email_arg="--register-unsafely-without-email" ;;
  *) email_arg="--email $email" ;;
esac

# Enable staging mode if needed
if [ $staging != "0" ]; then staging_arg="--staging"; fi

for domain in ${!domains[*]}; do
  domain_set=(${domains[$domain]})
  domain_name=`echo ${domain_set[0]} | grep -o -P $regex`

  if [ -e "$data_path/conf/live/$domain_name/cert.pem" ]; then
    echo "Skipping $domain_name domain"; else
    echo "### Deleting dummy certificate for $domain_name domain ..."
    rm -rf "$data_path/conf/live/$domain_name"
    echo "### Requesting Let's Encrypt certificate for $domain_name domain ..."

    #Join $domains to -d args
    domain_args=""
    for domain in "${domain_set[@]}"; do
      domain_args="$domain_args -d $domain"
    done

    mkdir -p "$data_path/www"
    docker-compose run --rm --entrypoint "certbot certonly --webroot -w /var/www/certbot --cert-name $domain_name $domain_args \
    $staging_arg $email_arg --rsa-key-size $rsa_key_size --agree-tos --force-renewal --non-interactive" certbot
  fi
done
```


#### Источники:
- [Попробуй эту инструкцию](https://gist.github.com/dancheskus/8d26823d0f5633e9dde63d150afb40b2) + читай комменты про замену адресов скрипта `init-letsencrypt.sh`
- [Ещё одна инструкция](https://dvsemenov.ru/nastrojka-https-s-pomoshhyu-nginx-lets-encrypt-i-docker/)
- [Образ certbot на docker hub](https://hub.docker.com/r/certbot/certbot)

#ssl #nginx #https #certbot