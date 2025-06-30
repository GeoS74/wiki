# Подключение gzip сжатия

На уровне `server`:

```
server {
 gzip on;
  gzip_min_length 1024;
  gzip_comp_level 6;
  gzip_types text/plain text/css application/json application/javascript text/xml text/javascript
  gzip_vary on;
  gzip_proxied any;
  gzip_disable "msie6";
  
location / {...}
}
```

#gzip