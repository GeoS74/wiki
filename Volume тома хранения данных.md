# Volume тома хранения данных


[Читать про volume](https://docs.docker.com/storage/volumes/)


[[Docker]] позволяет сохранять данные [[Контейнеры Docker|контейнеров]] в тома или монтировать каталоги хост машины к каталогам контейнеров. 

### Использование томов

Согласно документации, тома - это предпочтительный механизм для сохранения данных, созданных и используемых контейнерами Docker.

По сути, том - это каталог `/var/lib/docker/volumes` внутри Docker-а в котором сохраняются файлы контейнера.

Создать том можно вручную или налету. Пример:
```bash
docker run -v mydata:/app/log <image_name>
```

В данном случае будет создан том `mydata`, а по факту каталог в `/var/lib/docker/volumes/mydata`. Все файлы из контейнера будут скопированы в этот том.

Тома удаляются вручную. Это отдельная операция.

### Монтирование каталога

Для монтирования каталога хостмашины с каталогом контейнера надо указывать абсолютные адреса. Если на хост машине каталога не существует он будет создан. Пример:

```bash
docker run -v /var/lib/data:/app/log <image_name>
```

В этом случае на хост машине в каталоге `/var/lib/data` будут все файлы из `/app/log` и наоборот. Если контейнер стартует с привязкой к уже существующему локальному каталогу, он подхватывает все файлы внутри. Если изменить что-то в хост каталоге, то эти изменения сразу будут доступны в контейнере.

>Чтобы не заморачиваться с вводом относительного пути на хосте можно писать так: `-v "$(pwd)"/path:/app`

### Контроль привязок

Если используются привязки хранилища, то проинспектировать правильность установления привязок можно вызвав команду (см. ниже) и посмотреть объект `Mounts:{...}`:
```bash
sudo docker inspect <container_name>
```


#volume