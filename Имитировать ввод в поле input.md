# Имитировать ввод в поле input

Для некоторых сайтов простая вставка данных в `input` поле не гарантирует отправку данных формой на сервер. Это происходит потому, что формы отслеживают события на полях ввода, а затем сохраняют введённые значения в своём состоянии. Актуально для проектов написанных на [[React]].

Чтобы это обойти надо после вставки данных генерировать событие, причём событие обязательно должно всплывать (bubbles: true). Пример:

```js
// вставить данные в поле ввода
passInput.value = pass;
// создать всплывающее событие
const event = new Event("input", {bubbles: true});
// сгенерировать событие на поле ввода
passInput.dispatchEvent(event);
```

### Хитрый способ ввода

[Подсмотрел здесь](https://ru.stackoverflow.com/questions/1407669/%D0%9A%D0%B0%D0%BA-%D0%B8%D0%BC%D0%B8%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%B2%D0%B2%D0%BE%D0%B4-%D0%BF%D0%B0%D1%80%D0%BE%D0%BB%D1%8F-%D0%B2-input-password)

```js
// получить сеттер для инпута
const nativeSetter = Object.getOwnPropertyDescriptor(window.HTMLInputElement.prototype, "value").set;

// вставка значения в инпут
nativeSetter.call(input, value)

// генерация события на инпуте
const inputEvent = new Event("input", {bubbles: true});
input.dispatchEvent(inputEvent);
```

#хитрыйспособввода