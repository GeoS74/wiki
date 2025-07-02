

```js
const puppeteer = require('puppeteer');

(async () => {

  // Запуск браузера в видимом режиме
  const browser = await puppeteer.launch({
    headless: false,  // Отключаем headless-режим
    userDataDir: './user_data', // Сохраняем данные сессии (куки, кеш)
    args: ['--start-maximized'], // Максимизируем окно
    defaultViewport: null, // Используем размер окна браузера
    ignoreDefaultArgs: ['--disable-extensions'], // Не отключать расширения
    // executablePath: '/путь/к/chrome' // Укажите путь к Chrome, если нужно
  });

  const page = await browser.newPage();

  await page.goto('https://test.com');

  // Браузер не закроется автоматически
  console.log('Браузер открыт. Закройте его вручную.');

  // Для ручного закрытия (если нужно):
  // await browser.close();
})();
```