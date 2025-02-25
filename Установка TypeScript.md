# Установка [[TypeScript]]

##### [[Ubuntu]]

- [start project](https://nodejs.dev/en/learn/nodejs-with-typescript/)

Установка [[TypeScript]] не глобально:
```
npm i -D typescript
```

После установки пакетов вызов компилятора упал с ошибкой:
```
geos@geos-MacBookAir:~/nodejs/typescript$ tsc

Команда «tsc» не найдена, но может быть установлена с помощью:

sudo apt install node-typescript
```

После установки пакета `node-typescript` всё работает как надо.

При создании [[Контейнеры Docker|контейнеров]] [[Docker]] в скриптах файла `package.json`  для компиляции кода надо вызывать компилятор так: `npx tsc`.
_package.json:_
```json
"scripts": {
	"build": "npx tsc"
}
```

##### Windows
Установка [[TypeScript]] не глобально. В принципе всё тоже самое что и для [[Ubuntu]]:
```
npm i -D typescript
```

Но, компилятор тоже упадёт с ошибкой. Т.к. в Windows его надо вызывать так:
```
npx tsc
```

Чтобы можно было вызывать компилятор без `npx` надо установить [[TypeScript]] глобально:
```
npm i -g typescript
```

##### tsconfig.json

```
{
	"compilerOptions": {
		"module": "CommonJS",
		"noImplicitAny": true,
		"removeComments": true,
		"preserveConstEnums": true,
		"sourceMap": true
	},
	"include": ["."]
}


```

[Читать про компиляцию кода TypeScript](https://docs.microsoft.com/ru-ru/visualstudio/javascript/compile-typescript-code-npm?view=vs-2022)

```
{
  "compilerOptions": {
    "noImplicitAny": false,
    "noEmitOnError": true,
    "removeComments": false,
    "sourceMap": true,
    "target": "es5",
    "outDir": "dist"
  },
  "include": [
    "scripts/**/*"
  ]
}
```


Читай дальше [[Старт проекта]] на [[TypeScript]]

#tsconfig #typescriptinstall