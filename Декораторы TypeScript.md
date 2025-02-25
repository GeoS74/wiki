# Декораторы TypeScript


[Декораторы в документации](https://www.typescriptlang.org/docs/handbook/decorators.html)
[Полное руководство по декораторам](https://mirone.me/a-complete-guide-to-typescript-decorator/)

Для использования декораторов в [[TypeScript]] надо в файле `tsconfig.json` включить директиву:

```json
"compilerOptions": {
	...
	"target": "es2020",
	"experimentalDecorators": true,
}
```

> Прим.: версия в `target` должна быть `ES5` и выше.


#### Простые примеры

Пример простого декоратора для класса:
```ts
function simpleDecorator(target: any) {
  console.log('---hi I am a decorator---')
}

@simpleDecorator
class A {}
```

В объявлении функции-[[Декораторы класса|декоратора класса]] `simpleDecorator` обязательно должен быть один аргумент.

Пример с фабрикой декораторов:

```ts
function simpleDecorator(x: boolean) {
  console.log('---hi I am a decorator---')

  if(x) {
    return (target: any) => console.log('---function true---')
  }
  return (target: any) => console.log('---function false---')
}

@simpleDecorator(true)
class A {}
```

### [[Декораторы класса]]

### [[Пример шаблона декоратор на JS и TS]]