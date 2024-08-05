# Redis

[Документация](https://redis.io/docs/about/)
[Русскоязычная документация](https://github.com/rudocs/redis/blob/main/docs/index.md)
[Разбираемся с Redis](https://habr.com/ru/companies/wunderfund/articles/685894/)
[Шпаргалка по Redis](https://habr.com/ru/articles/204354/)

> Redis означает REmote DIctionary Server (англ. сервер удалённого словаря)


Есть 2 варианта: использовать Redis и использовать Redis-Stack. [[Image - образы Docker|Образов на hub.docker]] тоже несколько. [[NodeJS]] клиент [node-redis](https://github.com/redis/node-redis) использует образ [redis-stack-server](https://hub.docker.com/r/redis/redis-stack-server/) Подробнее про [[Использование официального образа redis-stack|использование официального образа redis-stack здесь.]]

### [[Установка redis на windows]]
### [[Использование официального образа redis-stack|Образ redis-stack]]

### [[Мониторинг метрик Redis]]

### [[Клиент node-redis]]

### redis vs redis-server

Как отмечено в [этом обсуждении](https://askubuntu.com/questions/1128572/redis-and-redis-server-packages) redis является мета-пакетом для redis-server. Это означает, что при установке на [[Ubuntu]] пакета redis будет установлен пакет redis-server.

[Взлом не защищённого экземпляра redis](http://antirez.com/news/96)





#Redis