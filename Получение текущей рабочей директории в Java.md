# Получение текущей рабочей директории в Java

```java
// может вывести ошибочный путь
String currentPath = new java.io.File(".").getCanonicalPath();
System.out.println(currentPath);

// правильный вариант
String currentDir = System.getProperty("user.dir");
System.out.println(currentDir);
```

В результате выполнения этого кода может быть получен вывод, который не соответствует ожидаемому. Например, вместо директории проекта может быть получена системная директория.

Это происходит потому, что `"."` обозначает текущую директорию в контексте процесса, запустившего [[Java]] приложение. Если приложение было запущено из системной директории, то именно она и будет возвращена.

[Подробнее](https://sky.pro/media/poluchenie-tekushhej-rabochej-direktorii-v-java/)