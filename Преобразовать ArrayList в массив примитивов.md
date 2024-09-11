# Преобразовать ArrayList в массив примитивов

С помощью стандартной библиотеки надо циклом прокатиться по списку и каждый элемент записать в массив, а можно более элегантно, но с использованием сторонней библиотеки. Пример:


```java
ArrayList<Byte> buf = new ArrayList<>();
buf.add((byte) 0);
buf.add((byte) 1);
byte[] foo = (byte[]) ArrayUtils.toPrimitive(buf.toArray());
```

Для такого преобразования надо импортировать стороннюю библиотеку: 
```
import org.apache.commons.lang3.*;
```

и добавить зависимость:
```xml
<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-lang3</artifactId>
	<version>3.17.0</version>
</dependency>
```


#Bytetobyte #ArrayListtoArray