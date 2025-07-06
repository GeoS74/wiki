# Кеширование в nginx

Для организации кеширования на уровне Nginx читай про [[Создание кастомного образа на основе nginx]]
##### Отключение кеширования для админки

```
location /admin/ {
    proxy_pass http://cms:80;
	# исключение кеширование для админки
	proxy_no_cache 1;
	proxy_cache_bypass 1;
  }
```


##### Настройка кеширования в основном nginx (nginx.conf или default.conf)

> При использовании [[Использование официального образа nginx | официального образа nginx]] нельзя указывать настройки на уровне http, но можно использовать на уровне server

```
server {
  listen 80;
  server_name ubp2a2.ru www.ubp2a2.ru;

  # путь для кеша
  proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=static_cache:10m inactive=60m use_temp_path=off;

  location ~\.js|css|ico|woff2|txt|svg|ttf$ {
    proxy_pass http://react:3333;
	
	# кеширование статики
	proxy_cache static_cache;
	proxy_cache_key "$scheme$host$request_uri";
	proxy_cache_valid 200 30d; # кешировать на 30 дней
	expires 30d; # браузерный кеш
	add_header Cache-Control "public, immutable";
	add_header X-Cache-Status $upstream_cache_status;
  }
}
```
 > это не сработает для уровня server!!! `proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=static_cache:10m inactive=60m use_temp_path=off;` надо определять на уровне http

`proxy_cache_path` - путь, где хранится кеш. папка должна существовать. (читай ниже как прокинуть её через docker-compose).

`levels=1:2` - структура подпапок для быстрого доступа.

`keys_zone=static_cache:10m` - имя зоны кеша и размер в RAM (10MB)

`inactive=60m` - удалять неиспользуемые кеши через 60 минут.

`proxy_cache` - включает кеширование для location

`proxy_cache_key` - определяет уникальный ключ кеша

`proxy_cache_valid 200 30d` - кешировать в Nginx на 30 дней

`expires 30d` - кешировать в браузере на 30 дней

`add_header Cache-Control "public, immutable"` - оптимизация для CDN и браузеров

##### Добавление тома для кеша в docker-compose.yml

```
 main:
    image: "nginx"
    ports:
      - "80:80"
    volumes:
      - ./nginx/front.conf:/etc/nginx/conf.d/default.conf
      - nginx_cache:/var/cache/nginx
volumes:
  nginx_cache:
```