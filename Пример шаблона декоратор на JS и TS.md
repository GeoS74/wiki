# Пример шаблона декоратор на JS и TS

Пример шаблона [[Декораторы TypeScript|Декоратор]] на чистом JS:

```js
function Coffee() {
  this.cost = () => 1
}

function Milk(coffee) {
  this.cost = () => coffee.cost() + 0.5;
}

function Sprinkles(coffee) {
  this.cost = () => coffee.cost() + 0.7;
}

console.log(new Sprinkles(new Milk(new Coffee())).cost()); // 2.2
```

Тот же пример с использованием [[Декораторы класса|декоратора класса]] на [[TypeScript]]:

```ts
function Sprinkles(target: any){
  return class extends target {
    cost(): number {
      return super.cost() + 0.7;
    }
  }
}

function Milk(target: any){
  return class extends target {
    cost(): number {
      return super.cost() + 0.5;
    }
  }
}

@Milk
@Sprinkles
class Coffee {
  cost() {
    return 1;
  }
}

console.log(new Coffee().cost()); // 2.2
```

Снова тот же пример, но теперь используя фабрику декораторов:

```ts
function Addition(ingredient: string) {
  let x = 0;

  switch(ingredient){
    case 'Milk': x = 0.5; break;
    case 'Sprinkles': x = 0.7; break;
  }

  return (target: any) => class extends target {
    cost(): number {
      return super.cost() + x;
    }
  }
}

@Addition('Milk')
@Addition('Sprinkles')
class Coffee {
  cost() {
    return 1;
  }
}
console.log(new Coffee().cost()); // 2.2
```