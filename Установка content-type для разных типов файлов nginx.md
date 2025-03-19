
# Установка content-type для разных типов файлов nginx

[[Nginx]] сопоставляет расширения запрашиваемых файлов и в зависимости от этого устанавливает заголовок `Content-type` [[HTTP]] . Если использовать [[Использование официального образа nginx|официальный образ nginx]], то файл с дефолтными типами находится в директории `/etc/nginx/mime.types`. Выглядит примерно так:

```
types {
    text/html                                        html htm shtml;
    text/css                                         css;
    text/xml                                         xml;
    image/gif                                        gif;
    image/jpeg                                       jpeg jpg;
    application/javascript                           js;
    application/atom+xml                             atom;
    application/rss+xml                              rss;
	...
}
```

В данном файле, к примеру нет типа `mjs`, так что при попытке запросить такой файл [[Nginx]] установить в `Content-Type` дефолтное значение. Скорее всего это будет `Content-Type: application/octet-stream` (см. пример ниже как переопределить значение по умолчанию). Для изменения этого поведения надо внести корректировку в конфигурацию.

```
server {
	listen 80;
	
	include /etc/nginx/mime.types;
	types {
		application/javascript js mjs;
	}

	#установка дефолтного значения (не обязательно)
	default_type application/octet-stream; 
}
```

Теперь все файлы `mjs` будут содержать правильный заголовок ответа.

>Прим.: можно эту инструкцию вносить на уровне `location{}`, можно на глобальном уровне `http{}`.

Подробнее читай в [документации](http://nginx.org/en/docs/http/ngx_http_core_module.html#types)

;) Вообще можно указать в качестве заголовка ответа все что угодно, например это будет возвращать прикольный заголовок `Content-Type`:

```
types {
		application/barracuda js;
	}
```



#nginx #конфигурацияnginx 