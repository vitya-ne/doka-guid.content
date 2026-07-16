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

Объявление переменной с помощью `using` связывает её с **синхронным** ресурсом. При выходе из блока, в котором переменная была определена, автоматически вызовется метод [Symbol.dispose](), освобождая ресурс. Для **асинхронных** ресурсов применяется синтаксис `using await` и вызывается метод `await [Symbol.asyncDispose]()`.


## Пример

Для имитации ресурса реализуем простой класс `FileResource`. Экземпляр класса имеет методы:
- `open()` — открыть файл;
- `read()` — читать данные файла;
- `close()` — закрыть файл;

```js
class FileResource {
  constructor(name) {
    this.name = name
  }

  open() {
    if (this._isOpen === undefined) {
      this._isOpen = true
    }
  }

  read() {
    if (this._isOpen) {
      return `данные ресурса ${this.name}`
    }
  }

  close() {
    if (this._isOpen) {
      this._isOpen = false
      console.info(`ресурс ${this.name} освобождён`)
    }
  }
}
```

Для работы с ресурсом используем блок [`try/catch/finally`](/js/try-catch/):

```js

function workWithResource() {
  let file
  try {
    file = new FileResource("file1.txt")
    // Используем ресурс
    file.open()
    console.log(file.read())
  } finally {
    // Освобождаем ресурс
    file?.close()
  }
}

workWithResource()
// данные ресурса file1.txt
// ресурс file1.txt освобождён

```