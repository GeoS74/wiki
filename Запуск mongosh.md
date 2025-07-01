# Запуск mongosh

[Подробнее про запуск mongosh](https://www.mongodb.com/docs/mongodb-shell/connect/#std-label-mdb-shell-connect)

Самый простой способ запустить [[MongoDB shell (mongosh)|mongosh]] - это выполнить на локальной машине:
```bash
mongosh
```

Что эквивалентно такой команде:
```bash
mongosh "mongodb://localhost:27017"
```

Хост и порт для подключения можно указать с помощью флагов:

```bash
mongosh --host localhost --port 27017
```

### Запуск mongosh в контейнере docker

Для запуска mongosh в [[Контейнеры Docker|docker контейнере]] сначала подключаемся к запущенному контейнеру, а затем:

```bash
mongosh -u <username> -p <password> --authenticationDatabase admin
```


#mongosh