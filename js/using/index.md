---
title: "using"
description: "Декларативный подход для описания ресурсов"
baseline:
  - group: explicit-resource-management
    features:
      - javascript.statements.using
      - javascript.builtins.Symbol.dispose
authors:
  - vitya-ne
related:
  - js/var-let
  - js/try-catch
  - js/generators
tags:
  - doka
---

## Кратко

Синтаксис `using` упрощает управление ресурсами, которые требуют явного освобождения после использования (например, сетевые соединения, потоки, подключения к базам данных).

**Синхронный** ресурс — это объект JavaScript с методом `[Symbol.dispose]()`. **Асинхронный** — с методом `[Symbol.asyncDispose]()`.

Объявление переменной с помощью `using` связывает её с **синхронным** ресурсом. При выходе из блока, в котором переменная была объявлена, автоматически вызовется метод `[Symbol.dispose]()`, освобождая ресурс. Для **асинхронных** ресурсов применяется синтаксис `await using` и вызывается метод `[Symbol.asyncDispose]()`.

## Пример

Рассмотрим простой пример использования `using`.

Создадим класс-обёртку для работы с экземпляром класса FileReader.

```js
  // класс-обёртка для FileReader
  class ManagedFileReader {
    constructor(file) {
      // Создание ресурса
      this.reader = new FileReader();
      console.log("FileReader экземпляр создан")
      this.file = file
    }

    // Метод использования ресурса
    read() {
      return new Promise((resolve, reject) => {
        this.reader.onload = () => resolve(this.reader.result)
        this.reader.onerror = reject
        this.reader.readAsText(this.file)
      })
    }

    // Метод освобождения ресурса
    close() {
      this.reader.abort()
      console.log("FileReader экземпляр освобождён")
    }
  }
```

Добавим метод `[Symbol.dispose]()`, вызывающий `close()`:

```js
  // класс-обёртка для FileReader
  class ManagedFileReader {
    // Существующая реализация
    // ...

    [Symbol.dispose]() {
      this.close()
    }
  }
```

Создадим функцию использования ресурса. Обратите внимание, что нам не нужно вызывать метод экземпляра 'close()` явно:

```js
  const processFile = async (file) => {
    using reader = new ManagedFileReader(file) // Создание
    const content = await reader.readAsText() // Использование
    console.log('Содержимое файла:', content)
  } // <-- Автоматическое освобождение при выходе

  processFile('readme.txt')
```

Посмотреть как происходит создание и освобождение ресурса c эффектом замедления можно с помощью демки.

<iframe title="Управление ресурсом с помощью using" src="demos/using-steps/" height="700"></iframe>

## Как пишется

```js
  using name = value
```

где:
- name — имя переменной, для ссылки на экземпляр ресурса;
- value — начальное значение переменной `name`. Значением переменной может быть: `null`, `undefined` или объект с методом `[Symbol.dispose]()`.

Для асинхронных ресурсов применяется синтаксис `await using`

```js
  await using name = value
```

где:
- value — начальное значение переменной `name`. Значением переменной может быть: `null`, `undefined`, объект с методом `[Symbol.asyncDispose]()` или `[Symbol.dispose]()`.

<aside>

☝️ Обратите внимание, что использование оператора [`await`](/js/async-await/) указывает на возможность выполнение асинхронной операции но не при объявлении переменной, а при освобождении ресурса.

</aside>

`using` можно использовать внутри:
- блока кода;
- функции;
- модуля;
- цикла `for` и `for..of`.

При попытке присвоить переменной недопустимое значение будет брошена ошибка TypeError.

## Как понять


