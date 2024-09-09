# Обход блокировки VPN

[Взято отсюда](https://habr.com/ru/articles/841460/)

Сразу скажу, обходной путь придумал не сам, мне его подсказал автор проекта zapret, а точнее его [комментарий](https://github.com/bol-van/zapret/issues/153#issuecomment-1717083035).

Добавлю, я использую nftables и nfqws, если этот вариант работает у меня — это не значит, что заработает и у вас! Возможно, вам придётся изменить некоторые параметры.

Первое, очень внимательно читаем комментарий по ссылке выше и штудируем, как портировать правила iptables в nftables.

```
$ iptables-translate -A OUTPUT -t mangle -o wlan0 -p udp --dport 443 -m mark ! --mark 0x40000000/0x40000000 -j NFQUEUE --queue-num 220 --queue-bypass
```

получилось:

```
$ nft 'add rule ip mangle OUTPUT oifname "wlan0" udp dport 443 mark and 0x40000000 != 0x40000000 counter queue num 220 bypass'
```

- wlan0 — интерфейс, через который мы ходим в сеть
- 443 — порт подключения к VPN сервису
- меняем OUTPUT на output

итоговая строка получается:

```
$ nft 'add rule ip mangle output oifname "wlan0" udp dport { 443, 18189 } mark and 0x40000000 != 0x40000000 counter queue num 220 bypass'
```

у меня два VPN коннекта, отсюда и два порта!

Перед запуском, проверим, что у нас есть таблица ip mangle:

```
$ nft list tables
```

если нет, создаём её и цепочку output

```
$ nft add table ip mangle$ nft 'add chain ip mangle output { type filter hook output priority 0 ; }'
```

Смотрим, что всё создалось:

```
$ nft -a list table ip mangle
```

теперь запускаем строку с добавлением нового правила и снова проверяем, что внутри таблицы.

и последнее, запускаем демон, который маскирует пакеты отправленные на требуемые порты

```
$ /opt/zapret/nfq/nfqws --uid 2 --qnum=220 --dpi-desync=fake --dpi-desync-any-protocol --dpi-desync-cutoff=d2 --dpi-desync-repeats=10 --dpi-desync-ttl=5 --daemon
```

обратите внимание, значение ttl у вас может отличаться. Своё я подглядел через `ps aux | grep nfqws`, которое стоит в демоне от zapret.

Понятно, что всё можно автоматизировать и не запускать всё руками, но мне надо было срочно. Самое главное, работает!