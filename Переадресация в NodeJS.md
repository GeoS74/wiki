# Переадресация в [[NodeJS]]

Точнее redirect в фреймворке Koa.

Redirect в Koa надо выполнять вместе  с передачей клиенту заголовка `Cache-Controk: no-cache`, иначе ответ сервера попадёт в кеш браузера и фактических обращений к серверу не будет, т.к. ответ в виде переадресации будет спокойно доставаться из кеша браузера
```
ctx.set('Cache-Control', 'no-cache');
ctx.status = 301;
ctx.redirect('/login');
```

