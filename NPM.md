# Менеджер пакетов NPM

>Интересный факт `npm` - это [не аббревиатура](https://twitter.com/npmjs/status/347057301401763840), но обычно его называют `node package manager` или `no problem man`


### Инициализация проекта
Это создаст файл `package.json`, причём при создании придётся ответить на вопросы:
```
npm init
```
Можно инициировать проект не отвечая на вопросы, установив значения по умолчанию:
```
npm init -y
```



### Установка пакетов
Флаг `--save-dev` (или `-D`) укажет NPM установить Typescript как **devDependency** . Разница между devDependency и зависимостью заключается в том, что devDependencies будут установлены только при запуске **npm install** , но не при установке пакета конечным пользователем.  
Например, Typescript нужен только при разработке пакета, но не нужен при использовании пакета.
### npm и npx

**npm** - инструмент для установки пакетов. Начиная с версии 5.2 в состав npm входит npx.

**npx** - инструмент для исполнения пакетов БЕЗ установки

Проверить установку npx:
```
npx cowsay hello
```

Подробнее [о различии между npm и npx](https://www.geeksforgeeks.org/what-are-the-differences-between-npm-and-npx/#:~:text=Npm%20is%20a%20tool%20that,pollution%20in%20the%20long%20term.)

К примеру можно создать проект [[React]] и удалить все файлы и пакеты, которые требовались только для установки

```
$ npx create-react-app my-app
```

Для сравнения, чтобы использовать `create-react-app` в npm, выполните следующие команды: 
```
$ npm install create-react-app
```
, а затем 
```
$ create-react-app myApp
```
(требуется установка).

### [[Создание пакета npm]]

### Флаги NPM install

-  `-P, --save-prod`: Пакет появится в вашем `dependencies`. Это является дефолтом, если только нет `-D`или `-O`Присутствующие.
-   `-D, --save-dev`: Пакет появится в вашем `devDependencies`.
-   `-O, --save-optional`: Пакет появится в вашем `optionalDependencies`.
-   `--no-save`: Предотвращает сберегательные к `dependencies`.

#npm