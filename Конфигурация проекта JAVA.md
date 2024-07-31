# Конфигурация проекта JAVA

В проектах [[Java]] в качестве файла конфигурации можно использовать файлы `*.properties*`, записывать в них значения в виде пар ключ->значение и считывать с использованием стандартного класса `Propertes`.

Пример:
файл `config.properties`:

```properties
node=dev
foo=bar
```

Чтение:
```java
public class Test {
    public static void main(String[] args) {
        File config = new File(Paths.get("./src/main/resources/config.properties").toString());
        
        Properties props = new Properties();
        
        props.load(new FileReader(config));
        
        System.out.println(props.get("node"));
    }
}
```


См. также [[Свойства системы|как вывести все свойства класса System]].

#propertiesjava #configjava