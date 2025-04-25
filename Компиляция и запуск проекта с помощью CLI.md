
# Компиляция и запуск проекта с помощью CLI

Для начала надо скачать библиотеку [[JavaFX]] с официального сайта проекта.

Команды писать в одну строку, здесь переносы установлены для наглядности.

Компиляция проекта:

```bash
javac 
	-encoding utf-8 
	--module-path /home/geos/javafx/javafx-sdk-24.0.1/lib 
	--add-modules javafx.controls 
	-d /home/geos/javafx/test /home/geos/javafx/test/*.java
```

После компиляции появится файл с классом. Причем если, к примеру пакет называется `application`, а главный класс `App`, то файл с классом появится в директории `/home/geos/javafx/test/application/App.class`.

Запуск файла класса:

```bash
java 
	--module-path /home/geos/javafx/javafx-sdk-24.0.1/lib 
	--add-modules javafx.controls 
	-cp /home/geos/javafx/test application.App
```

Сборка `jar` файла (из директории `/home/geos/javafx/test`):

```bash
jar 
	cvfe /home/geos/javafx/test/foo/Snake.jar application.App 
	-C /home/geos/javafx/test .
```

Запуск `jar` файла:

```bash
java 
	--module-path /home/geos/javafx/javafx-sdk-24.0.1/lib 
	--add-modules javafx.controls 
	-jar ./foo/Snake.jar
```