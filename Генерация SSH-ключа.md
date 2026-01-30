# Генерация SSH-ключа

[статья на github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

1) генерация SSH-ключа
```shell
$ ssh-keygen -t ed25519 -C "your_email@example.com"

# расширенный вариант
$ ssh-keygen -t ed25519 -f github_actions_key -N "" -C "comment"
# -t ed25519 - тип ключа
# -f github_actions_key - имя файла для ключа
# -N "" - парольная фраза — в данном примере пустая
# -C "comment" - комментарий
```


>**Примечание.** Если вы используете устаревшую систему, которая не поддерживает алгоритм Ed25519, используйте:
```shell
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

2) добавление вашего SSH-ключа в ssh-agent
```
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_ed25519
```

3) копирование открытого ключа SSH в буфер обмена
[статья на github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
```
$ clip < ~/.ssh/id_ed25519.pub
```

В  Ubuntu не удалось воспользоваться командой `clip`, пришлось использовать утилиту `xclip` (естественно, предварительно её установив). Как я настраивал без этого использование SSH ключей для GitHub не помню...
```
$ xclip -sel clip < ~/.ssh/id_rsa.pub
```

Читай про [[Доступ к серверу по SSH|доступ к серверу по SSH]]