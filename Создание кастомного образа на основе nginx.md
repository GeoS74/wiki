# Создание кастомного образа на основе nginx

[[Использование официального образа nginx]] не позволяет прокидывать конфигурацию на уровне  `http`, только на уровне `server`. Для решения это проблемы можно собрать свой образ на основе [[Nginx]].

Основная конфигурация официального образа nginx находится в файле `/etc/nginx/nginx.conf`  и выглядит примерно так:

```
user nginx;

worker_processes auto;

error_log /var/log/nginx/error.log notice;

pid /var/run/nginx.pid;

events {
	worker_connections 1024;
}

include /etc/nginx/http.conf.template;

http {
	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	log_format main '$remote_addr - $remote_user [$time_local] "$request" '
	'$status $body_bytes_sent "$http_referer" '
	'"$http_user_agent" "$http_x_forwarded_for"';
	access_log /var/log/nginx/access.log main;

	sendfile on;
	#tcp_nopush on;
	
	keepalive_timeout 65;
	#gzip on;
	
	include /etc/nginx/conf.d/*.conf;
}
```

  Т.е. надо выдернуть из этой конфигурации блок http и заменить его своим.

#### Шаг 1 - изменяем nginx.conf

В файле `nginx.conf`  пишем следующее:

```
user nginx;

worker_processes auto;

error_log /var/log/nginx/error.log notice;

pid /var/run/nginx.pid;

events {
  worker_connections 1024;
}

# подключаем свою конфигурацию блока http
include /etc/nginx/http.conf.template;
```

#### Шаг 2 - собираем образ

Файл Dockerfile:

```
FROM nginx:latest
COPY ./nginx.conf /etc/nginx/nginx.conf
```

#### Шаг 3 - создаём файл конфигурации http блока

Создаём файл `http.conf.template` и пишем в него свою конфигурацию, можно добавить поддержку кеширования или что-то ещё. Минимально она выглядит так:

```
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

#### Шаг 4 - добавляем конфигурацию на уровне server и запускаем

 Создаём конфигурацию блока `server`, файл `admin.conf`:

```
server {

listen 7000;

location / {
	root /usr/share/nginx/html;
	try_files $uri /index.html;
}
}
```

Файл `docker-compose.yml`:

```yml
version: '1.0'

services:
	main:
		image: "nginx-custom"
			ports:
				- "8000:7000"
		volumes:
			# прокидываем свою конфигурацию http блока
			- ./http.conf.template:/etc/nginx/http.conf.template
			# основная конфигурация
			- ./admin.conf:/etc/nginx/conf.d/default.conf
```

Теперь если запустить этот код, сервер [[Nginx]] будет доступен на порту 8000.