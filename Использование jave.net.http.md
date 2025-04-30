# Использование jave.net.http

При использовании пакета `jave.net.http` в простом приложении [[Java]] пакет импортируется в проект без проблем. Но, при создании проекта [[JavaFX]], чтобы использовать этот пакет надо прописать его в модулях, файл `module-info.java`. Пример:

```java
module com.geos.salesman {
    requires javafx.controls;
    requires java.net.http;
    exports com.geos.salesman;
}
```