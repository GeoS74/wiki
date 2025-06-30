# Подключение gzip сжатия


Проверка поддержки Gzip:

```bash
nginx -V 2>&1 | grep -o http_gzip
```

Если вывод есть, значит Gzip доступен.


На уровне `server`:

```
server {
  # включение Gzip
  gzip on;

  # минимальный размер файла для сжатия (1КБ)
  gzip_min_length 1024;

  # уровень сжатия (1-9, 6 - оптимально)
  gzip_comp_level 6;

  # типы файлов для сжатия
  gzip_types text/plain text/css application/json application/javascript text/xml text/javascript

  # добавляет заголовок "Vary: Accept-Encoding" для кеширования
  gzip_vary on;

  # проксируемые запросы тоже сжимать 
  gzip_proxied any;

  # отключает сжатие для старых версий IE (6 и ниже)
  gzip_disable "msie6";
  
  location / {...}
}
```

#gzip