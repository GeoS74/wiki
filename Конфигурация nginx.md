# Конфигурация nginx

[Пример файла конфигурации из документации](http://nginx.org/en/docs/example.html)

Для конфигурирования [[Nginx]] в директории `/etc/nginx` есть основной файл конфигурации `nginx.conf`.  К этому файлу с помощью инструкции `include` можно подключать свои конфигурации, которые должны быть расположены в директории `/etc/nginx/conf.d`.

Файлы конфигурации должны иметь имя вида: `<name_file>.conf`

Например в папке с конфигурациями есть файлы:
- `my_conf_1.conf`
- `my_conf_2.conf`

Файл основной конфигурации будет примерно таким:

```
http {
	...
	
	# чтобы подключить все файлы конфигурации
    include /etc/nginx/conf.d/*.conf;

	# подключить определённые файлы конфигурации
	include /etc/nginx/conf.d/my_conf_1.conf
	include /etc/nginx/conf.d/my_conf_2.conf
}

```

[Руководство для начинающих](https://nginx.org/ru/docs/beginners_guide.html)

[Подробнее в документации](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/)

Читай про [[Использование официального образа nginx | конфигурирование официального образа.]]

### [[Установка content-type для разных типов файлов nginx]]