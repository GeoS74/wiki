
# Компиляция и запуск проекта с помощью CLI

Для начала надо скачать библиотеку [[JavaFX]] с официального сайта проекта.

Команды писать в одну строку, здесь переносы установлены для наглядности.

В качестве примера имеем такую структуру файлов:

`/home/geos/javafx/test/scr/App.java` - файл с основным классом приложения [[Java]]

`/home/geos/javafx/test/image` - каталог с картинками, используемыми приложением

Пакет приложения: `com.geos.snake2`

Все команды запускаются из `/home/geos/javafx/test`

##### Компиляция проекта:

```bash
javac 
	-encoding utf-8 
	--module-path /home/geos/javafx/javafx-sdk-24.0.1/lib 
	--add-modules javafx.controls 
	-d /home/geos/javafx/test/build /home/geos/javafx/test/src/*.java
```

После компиляции будет создан каталог `build` с вложенной структурой каталогов, соответствующей названию пакета. В данном случае будет создан файл с байт-кодом: `/home/geos/javafx/test/build/com/geos/snake2/App.class`

##### Запуск файла класса:

```bash
java 
	--module-path /home/geos/javafx/javafx-sdk-24.0.1/lib 
	--add-modules javafx.controls 
	-cp /home/geos/javafx/test/build/ com.geos.snake2.App

```

> Прим.: несмотря на то, что фактически файл с байт кодом лежит в директории `home/geos/javafx/test/build/com/geos/snake2` запустить его так: `-cp home/geos/javafx/test/build/com/geos/snake2 App` не получится! Скорее всего это связано с объявлением пакета в исходном файле


##### Сборка `jar` файла:

```bash
jar 
	cvfe /home/geos/javafx/test/snake.jar com.geos.snake2.App 
	-C /home/geos/javafx/test/build .
```

> Прим.: в конце команды есть точка! Это важно. Если указать что-то другое то приложение не запуститься. Не уверен, но скорее всего это работает по аналогии со сборкой контейнеров в [[Docker]], и эта точка указывает домашнюю директорию внутри `.jar` файла. К примеру вот так `-C /home/geos/javafx/test ./build` jar-файл будет создан, но при запуске упадёт с ошибкой

Если надо добавить в `.jar` файл какие-то файлы, к примеру каталог с изображениями, то надо после точки добавить пути добавляемых директорий. Пример, добавляем каталог с изображениями из директории `image/` и файлы библиотеки из `lib/`:

```bash
jar 
	cvfe /home/geos/javafx/test/snake.jar com.geos.snake2.App 
	-C /home/geos/javafx/test/build . image/ lib/
```


> Проверить структуру `.jar` файла можно вызвав команду `jar tf app.jar`

##### Запуск `jar` файла:

```bash
java 
	--module-path /home/geos/javafx/javafx-sdk-24.0.1/lib 
	--add-modules javafx.controls 
	-jar snake.jar
```


##### Замечания

- [[Проблемы загрузки изображений в JavaFX приложениях]]


## Альтернативный вариант создания .jar-файла

Перед запуском следующих примеров надо создать в корневой директории каталог `lib` и скопировать в него файлы библиотеки [[JavaFX]] из `/home/geos/javafx/javafx-sdk-24.0.1/lib`. Идея в том, чтобы создать `fat jar`, т.е. в `.jar`-файл упаковать библиотеку [[JavaFX]] для того, чтобы можно было запускать его без указания пути к внешней библиотеке.

##### Сборка `jar` файла с манифестом:

В корневую директорию добавить файл `MANIFEST.MF` со следующим содержимым:

```mf
Manifest-Version: 1.0
Main-Class: com.geos.snake2.App
<пустая строка>
```


> Важно: в конце файла `MANIFEST.MF` должна быть пустая строка. После сборки можно проверить содержимое файла MANIFEST выполнив: `unzip -p app.jar META-INF/MANIFEST.MF`

Сборка (здесь изменёно название `jar`   файла, чтобы не путать с предыдущими примерами):

```bash
jar cvfm app.jar META-INF/MANIFEST.MF -C /home/geos/javafx/test/build . image/ lib/
```

В данном случае в `jar` упаковывается библиотека [[JavaFX]]  директория с изображениями.

##### Запуск `jar` файла:

Если библиотека [[JavaFX]] подключается с текущей хост-машины:

```bash
java --module-path /home/geos/javafx/javafx-sdk-24.0.1/lib --add-modules javafx.controls -jar app.jar
```

Если библиотека [[JavaFX]] запускается из `jar` файла (папка `lib/`):

```bash
java --module-path "lib/" --add-modules javafx.controls -jar app.jar
```

