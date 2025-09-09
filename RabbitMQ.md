
# RabbitMQ

[Документация](https://www.rabbitmq.com)

### [[Простой пример использования очереди RabbitMQ]]

### Разное
##### Ограничение по количеству сообщений

```js
// Установка лимита в 10000 сообщений
await channel.assertQueue('my_queue', {
  durable: true,
  maxLength: 10000  // Максимум сообщений в очереди
});
```

##### Ограничение по объёму памяти (bytes)

```js
await channel.assertQueue('my_queue', {
  durable: true,
  maxLengthBytes: 104857600  // 100 MB
});
```

##### Политика истечения срока (TTL)

```js
// Сообщения удаляются через 1 час
await channel.assertQueue('my_queue', {
  durable: true,
  messageTtl: 3600000  // 1 час в миллисекундах
});
```

##### 📏 Как получить размер очереди

```js
const amqp = require('amqplib');

async function getQueueInfo() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();
  
  const queue = 'test_queue';
  const queueInfo = await channel.assertQueue(queue);
  
  console.log('📊 Информация об очереди:');
  console.log('- Имя очереди:', queueInfo.queue);
  console.log('- Сообщений в очереди:', queueInfo.messageCount);
  console.log('- Consumers подключено:', queueInfo.consumerCount);
  
  await connection.close();
}

getQueueInfo();
```


#RabbitMQ