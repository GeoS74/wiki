# Populate

### [[Самовложенный список]]

### Простой пример

Пример, есть две модели. Первая модель - это пользователь, который может иметь несколько ролей. Роли - это вторая модель, которая хранит некие вложенные данные.

Модель пользователя:
```js
const UserSchema = new mongoose.Schema({
  roles: {
    type: [mongoose.Schema.Types.ObjectId],
    ref: Role,
  },

  email: String,
});
```


Модель роли:

```js

// вспомогательная схема для связки задача->действие
const TaskToActions = new mongoose.Schema({
  task: {
    type: mongoose.Schema.Types.ObjectId,
    ref: Task,
  },

  actions: {
    type: [mongoose.Schema.Types.ObjectId],
    ref: Action,
  },
});

// вспомогательная схема для связки направление->задача
const DirectingToTasks = new mongoose.Schema({
  directing: {
    type: mongoose.Schema.Types.ObjectId,
    ref: Directing,
  },

  tasks: [TaskToActions],
});

// основная схема роли
const RoleSchema = new mongoose.Schema({
  title: {
    type: String,
    unique: 'Не уникальное значение {PATH}',
  },
  directings: [DirectingToTasks],
});
```

Модели Task, Action и Directing выглядят одинаково:

```js
const Schema = new mongoose.Schema({
  title: String,
});
```


### Использование `populate` при запросе к User:

```js
User.find({})
.populate({
      path: 'roles',
      populate: [
        { path: 'directings.directing' },
        { path: 'directings.tasks.task' },
        { path: 'directings.tasks.actions' },
      ],
    });
```


### Использование `populate` при запросе к Role:

```js
Role.find({})
    .populate('directings.directing')
    .populate('directings.tasks.task')
    .populate('directings.tasks.actions');
```


#populate