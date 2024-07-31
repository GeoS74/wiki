# Простой HTTP сервер на Java

[[Java]] позволяет написать простой http сервер, но в отличие от [[NodeJS]] здесь придётся руками обрабатывать вообще всё. В примере ниже создаётся сокет, затем он ожидает подключения, потом создаёт потоки чтения/записи. Потом читает обращение и... подвешивает соединение. Для корректного завершения чтения обращения, надо правильно обрабатывать запросы в соответствии с документацией протокола [[HTTP]].

```java
package ru.sgn74.httpserver;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class HttpServer {
    private static int port = 8000;
            
    public static void main(String[] args) throws IOException {
        try(ServerSocket server = new ServerSocket(port)) {
            System.out.println("server started at "+port);
            
            while(true) {
                Socket client = server.accept();
                System.out.println("Client connected!\n");
                
                try(
                    BufferedReader in = new BufferedReader(new InputStreamReader(client.getInputStream()));
                    BufferedWriter out = new BufferedWriter(new OutputStreamWriter(client.getOutputStream()))
                ) {
                    String message;
                    while((message = in.readLine()) != null) {
                        System.out.println(message);
                    }
                    
                    System.out.println("end read line");
                    
                    out.write("HTTP/1.1 201 OK\n");
                    out.write("Content-Type: text/html; charset=utf-8\n");
                    out.write("\n");
                    out.write("<p>hello world</p>");
                    out.flush();
                }
                
                client.close();
            }
            
        } 
        catch(IOException e) {
            System.err.println(e.getMessage());
        }
    }
}
```

Этот сервер подвесит обращение клиента, т.к. не сможет корректно обработать поступивший запрос. Т.к. для разных методов `GET`, `POST` и т.д. тело запроса быглядит по разному. Например `GET` запрос будет выведен в консоль так:

```
GET / HTTP/1.1
Host: localhost:8000
User-Agent: curl/8.7.1
Accept: */*

```
В конце будет пустая строка, поэтому для `GET` запросов можно было бы завершить цикл чтения так:

```
while((message = in.readLine()) != null) {
  if(message.isEmpty()) break;
  System.out.println(message);
}
```

Для `POST` запроса такой трюк не сработает, т.к. там могут быть пустые строки. Пример:

посылаем запрос с помощью [[curl]]:

```bash
curl -X POST -F name="geos" http://localhost:8000
```

в консоли получаем:
```
POST / HTTP/1.1
Host: localhost:8000
User-Agent: curl/8.7.1
Accept: */*
Content-Length: 155
Content-Type: multipart/form-data; boundary=------------------------XoLCwCzZgknAD1iXC2NVX9

--------------------------XoLCwCzZgknAD1iXC2NVX9
Content-Disposition: form-data; name="name"

geos
--------------------------XoLCwCzZgknAD1iXC2NVX9--

```

Почитать про [Протокол HTTP/1.1 - перевод на русский](https://www.opennet.ru/docs/RUS/http11/)

## Клиент к серверу

```java
package ru.sgn74.echoclient;
import java.io.*;
import java.net.Socket;

public class EchoClient {
    private static Socket clientSocket;
    private static BufferedReader reader;
    private static BufferedReader in;
    private static BufferedWriter out;
    
    public static void main(String[] args) {
        int portNumber = 8000;
        try {
            try {
                clientSocket = new Socket("localhost", portNumber);
                reader = new BufferedReader(new InputStreamReader(System.in));
                in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
                out = new BufferedWriter(new OutputStreamWriter(clientSocket.getOutputStream()));

                while(true) {
                    System.out.println("Вы что-то хотели сказать? Введите это здесь:");
                    String word = reader.readLine();
                    out.write(word + "\n");
                    out.flush();
                    String serverWord = in.readLine();
                    System.out.println(serverWord);
                }
            } finally { // в любом случае необходимо закрыть сокет и потоки
                System.out.println("Клиент был закрыт...");
                clientSocket.close();
                in.close();
                out.close();
            }
        } catch (IOException e) {
            System.err.println(e);
        }
    }
}
```