# Добавление нескольких url в origin

Заголовок `# Access-Control-Allow-Origin` позволяет указать домен для доступа к ресурсу. Также можно указать звёздочку `*`, чтобы разрешить обращение от всех доменов.  Можно ещё  указать `null`, но так делать [не рекомендуется](https://w3c.github.io/webappsec-cors-for-developers/#avoid-returning-access-control-allow-origin-null)

Если необходимо в заголовок `Origin` добавить несколько доменов, то вариант с указанием их через запятую не сработает. Используя пакет @koa/cors для решения этой проблемы можно написать так:

```js
app.use(cors({
  origin(ctx) {
    switch(ctx.get('Origin')) {
	  // перечисляем все url
      case 'http://google.com':
      case 'http://localhost:4000':
        return 'ctx.get('Origin')';
        break;
      default: // возвращаем дефолтное значение
        return '';
    }
  },
}))
```

Т.к. в качестве дефолтного значения `null` указывать не следует, то возвращается просто пустая строка.

#cors