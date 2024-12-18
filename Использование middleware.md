# Использование middleware

[Документация](https://mongoosejs.com/docs/middleware.html)

Задача: используя middleware надо записывать и обновлять поле при каждом сохранении или изменении документа.

Решение:

```js
const Schema = new mongoose.Schema({
  title: String,
  article: String,
  alias: String, // поле, которое надо обновить
});

// заполнение alias перед созданием
Schema.pre('save', function s() {
  this.alias = `${this.article}_${this.title}`;
});

// изменение alias при редактировании документа
Schema.pre('findOneAndUpdate', function u() {
  // получить данные
  const update = this.getUpdate();
  const alias = `${update.article}_${update.title}`;
  // обновить данные
  this.setUpdate({ ...update, alias });
});
```

Интересно, что при изменении документа свойства документа не доступны по `this`, вместо этого надо использовать функции `this.getUpdate()` и `this.setUpdate()`.

[Читай про доступ к параметрам](https://mongoosejs.com/docs/middleware.html#accessing-parameters-in-middleware)