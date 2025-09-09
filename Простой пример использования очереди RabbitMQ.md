# Простой пример использования очереди RabbitMQ


Файл `producer.js` - добавляет задачи в очередь:

```js
const amqp = require('amqplib');

async function produceMessage() {
  try {
    // Подключаемся к RabbitMQ
    const connection = await amqp.connect('amqp://localhost:5672');
    const channel = await connection.createChannel();

    // Создаем очередь (если не существует)
    const queue = 'test_queue';
    await channel.assertQueue(queue, {
      durable: true // Сообщения сохраняются при перезагрузке
    });

    const messages = new Array(5).fill('text');

    for (let i = 0; i < messages.length; i++) {
      const message = `message ${i+1}`;
      channel.sendToQueue(queue, Buffer.from(message), {
        persistent: true // Сообщение сохраняется на диск
      });

      console.log(`✅ Отправлено: "${message}"`);
      await delay(3000);
    }

    // Закрываем соединение через 500ms
    setTimeout(() => {
      connection.close();
      console.log('🔌 Соединение закрыто');
      process.exit(0);
    }, 500);

  } catch (error) {
    console.error('❌ Ошибка:', error.message);
  }
}
produceMessage();

function delay(ms) {
  return new Promise(res => {
    setTimeout(() => res(1), ms)
  })
}
```

Файл `consumer.js` - обрабатывает задачи из очереди:

```js
const amqp = require('amqplib');

async function consumeMessages() {
  try {
    // Подключаемся к RabbitMQ
    const connection = await amqp.connect('amqp://localhost:5672');
    const channel = await connection.createChannel();

    // Создаем очередь (такая же как в producer)
    const queue = 'test_queue';
    await channel.assertQueue(queue, {
      durable: true
    });

    // получаем задачи по 1 за раз
    channel.prefetch(1);
    console.log('🔄 Ожидаем сообщения... Для выхода: Ctrl+C');

    // Настраиваем потребителя
    channel.consume(queue, (message) => {
      if (message !== null) {
        const content = message.content.toString();
        console.log(`📨 Получено: "${content}"`);

        // Имитируем обработку
        setTimeout(() => {
          console.log(`✅ Обработано: "${content}"`);
          channel.ack(message); // Подтверждаем обработку
        }, 3000);
      }
    }, {
      noAck: false // Требуем подтверждения обработки
    });

    // Обработка закрытия приложения
    process.on('SIGINT', () => {
      console.log('\n🔌 Закрываем соединение...');
      connection.close();
      process.exit(0);
    });
  } catch (error) {
    console.error('❌ Ошибка:', error.message);
  }
}
consumeMessages();
```


Файлов `consumer.js` как и процессов может быть запущено несколько. Если обработчиков задач несколько, то `channel.prefetch(1);` заставит их брать по 1 задаче за раз.
