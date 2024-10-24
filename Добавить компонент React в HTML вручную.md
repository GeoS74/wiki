# Добавить компонент React в HTML вручную

При таком подходе к html странице подключаются две библиотеки [[React]]. Собственно сам React и ReactDOM. Смотри правильно работающий пример в самом низу этой заметки.


Исходный html файл:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
</head>
<body>
  <div id="menu"></div>
  <div id="component"></div>
  <script src="component.js"></script>
  <script src="menu.js"></script>
</body>
</html>
```

Далее пишем [[React]] компоненту (файл `component.js`) с использованием JSX:

```js
const MyComponent = (
  <div>
    <h1>Blog Title</h1>
    <p>Here is the blog description.</p>
  </div>
  );

ReactDOM.render(MyComponent, document.getElementById("component"));
```

Теперь, чтобы транспилировать JSX в JS используем babel.

```bash
npm i @babel/cli
npm i @babel/preset-react
```

Запуск транспиляции:

```bash
npx babel --presets @babel/preset-react ./src/component.jsx -o ./build/component.js
```


### Ручная транспиляция JSX

##### Вариант 1:
Добавить команды в `package.json`

```json
"scripts": {
    "build": "babel --presets @babel/preset-react ./src/component.jsx -o ./build/component.js & babel --presets @babel/preset-react ./src/menu.jsx -o ./build/menu.js"

  },
```

Если файлов много, то это будет очень громоздкая конструкция.

##### Вариант 2:
Написать свой билдер на [[NodeJS]].

```js
// здесь можно обойти всю папку src в цикле и обработать все файлы JSX
const childProcess = require('child_process');

childProcess.execSync('npx babel --presets @babel/preset-react ./src/component.jsx -o ./build/component.js');
```

### Замечания

[[React]] ругается в консоль, говоря что метод `ReactDOM.render` объявлен устаревшим. Предлагается начать использовать пакет `react-dom/clien`. Причём подключать его как модуль:

```
import { createRoot } from 'react-dom/client';
const container = document.getElementById('app');
const root = createRoot(container); 
root.render(<App tab="home" />);
```
Т.к. в этом примере используется подключение библиотеки [[React]] через cdn сразу в html страницу, нет возможности импортировать 'react-dom/client'. А сама библиотека, полученная через cdn также не содержит в себе 'react-dom/client'. Возможно это добавят в будущих сборках.

### Решено  - правильное использование

```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
</head>
<body>
  <div id="component"></div>
  <script src="component.js" type="module"></script>
</body>
</html>
```

> ВАЖНО: подключаемый скрипт должен быть обозначен как `type="module`, чтобы внутри .jsx компонента можно было выполнять export/import.

```jsx
function App() {
  const [count, setCount] = React.useState(0);
  
  return <div onClick={() => setCount(count + 1)}>
    count: {count}
  </div>
}

const root = ReactDOM.createRoot(document.getElementById("component"));
root.render(<App />)
```


#babel #react-dom