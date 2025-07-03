
# Проблема с использованием URL.parse() при SSR

Сайт для разбора URL в своём коде использовал `URL.parse()`, при этом [[Puppeteer]] напрочь отказывался отрисовывать страницу. После того, как я запустил его с визуализацией Chrome, то в консоли обнаружил ошибку. 

## **Почему возникает ошибка?**

1. **Устаревший метод**: `URL.parse()` был частью Node.js API, но в браузерах используется стандартный конструктор `URL`.
    
2. **Разные окружения**: Страница ожидает Node.js-окружение, но выполняется в браузере.
    
3. **Puppeteer использует современный Chrome**: В новых версиях Chrome `URL.parse` не существует, только `new URL()`.
4. 

## Возможные варианты решения:

Вариант 1: модифицировать страницу

```
// Было:
const parsedUrl = URL.parse("https://example.com");

// Стало:
const parsedUrl = new URL("https://example.com");
```

Вариант 2: добавить полифил через [[Puppeteer]]

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({ headless: false });
  const page = await browser.newPage();

  // Вставляем полифил для URL.parse
  await page.evaluateOnNewDocument(() => {
    if (typeof URL.parse === 'undefined') {
      URL.parse = function (url) {

      try {
        // Если URL валидный - парсим стандартным способом
        const parsed = new URL(url);
        return {
          protocol: parsed.protocol,
          hostname: parsed.hostname,
          pathname: parsed.pathname,
          query: parsed.searchParams,
          href: parsed.href,
          origin: parsed.origin,
          port: parsed.port,
          hash: parsed.hash
        };
      } catch (e) {
        // Фоллбек для невалидных URL
        //Может быть такая ошибка TypeError: Failed to construct 'URL': Invalid URL
        const dummyLink = document.createElement('a');
        dummyLink.href = url;
        return {
          protocol: dummyLink.protocol,
          hostname: dummyLink.hostname,
          pathname: dummyLink.pathname,
          query: new URLSearchParams(dummyLink.search),
          href: dummyLink.href,
          origin: dummyLink.protocol + '//' + dummyLink.host,
          port: dummyLink.port,
          hash: dummyLink.hash
        };
      }
    };
    }
  });

  await page.goto('https://ваш-сайт.com');
  await page.waitForTimeout(5000); // Даем время для проверки
  await browser.close();
})();
```

При использовании [[TypeScript]] вариант такой:

```ts
await page.evaluateOnNewDocument(() => {
  if (!('parse' in URL)) {
    (URL as any).parse = function(url: string) {
      // тут код полифила
    };
  }
});
```

Пояснения:

`evaluateOnNewDocument()` Выполняет JavaScript-код **до того**, как страница начнёт загружаться.  Это гарантирует, что метод `URL.parse` будет доступен сразу при загрузке страницы.

`if (!('parse' in URL))` - Убеждаемся, что метод `parse` ещё не существует в объекте `URL`.  Это предотвращает перезапись существующего метода (если вдруг он уже есть).

`(URL as any)` Проблема:  TypeScript знает, что стандартный `URL` не имеет метода `parse`, и выдаст ошибку.  Решение: `(URL as any)` — временно отключает проверку типов для `URL`, позволяя добавить кастомный метод.


### Полифил для Promise.withResolvers

Дополнительно ещё один полифил

```
if (!Promise.withResolvers) {
      Promise.withResolvers = function () {
        let resolve;
        let reject;

        const promise = new Promise((res, rej) => {
          resolve = res;
          reject = rej;
        });

        return { promise, resolve, reject };
      };
}
```


#puppeteer #URL.parse