# cat

[Почитать про cat](https://losst.pro/komanda-cat-linux#%D0%9A%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B0_cat)

## Команда cat

Название команды - это сокращения от слова catenate. По сути, задача команды cat очень проста - она читает данные из файла или стандартного ввода и выводит их на экран. Это все, чем занимается утилита. Но с помощью ее опций и операторов перенаправления вывода можно сделать очень многое, например [[Вставить текст в файл Ubuntu]] . Сначала рассмотрим синтаксис утилиты:

**cat <span style="color:orange">опции</span> файл1 файл2**

Вы можете передать утилите несколько файлов и тогда их содержимое будет выведено поочередно, без разделителей. Опции позволяют очень сильно видоизменить вывод и сделать именно то, что вам нужно. Рассмотрим основные опции:

- **-b** - нумеровать только непустые строки;
- **-E** - показывать символ $ в конце каждой строки;
- **-n** - нумеровать все строки;
- **-s** - удалять пустые повторяющиеся строки;
- **-T** - отображать табуляции в виде ^I;
- **-h** - отобразить справку;
- **-v** - версия утилиты.

## Вставить текст в файл с помощью cat

Классный вариант (сработал внутри [[Контейнеры Docker|контейнера docker]], где не было   `nano`):
```bash
$ cat > myfyle.txt <<EOF
lorem
ipsum
EOF
```

Ещё один не плохой вариант:

```bash
$ cat >> myfile.txt
lorem ipsum
^D
```

где ^D - CTRL+D

Ещё больше классных вариантов [здесь](https://linux-notes.org/vstavit-tekst-v-fajl-v-unix-linux/)



#cat #cli