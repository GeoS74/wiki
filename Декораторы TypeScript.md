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