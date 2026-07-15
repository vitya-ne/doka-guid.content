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

Синтаксис `using` упрощает управление ресурсами (например, сетевые соединения, потоки, подключения к базам данных), которые требуют явного освобождения после использования.

Ресурс — это любой объект JavaScript с методом `[Symbol.dispose]()` (для синхронного освобождения) или `[Symbol.asyncDispose]()` (для асинхронного), например, закрытие соединения или отключение от базы данных.

Объявление переменной с помощью `using` создаёт ссылку на ресурс. При выходе из блока, в котором объявлена переменная,  автоматически вызывается метод `[Symbol.dispose]()`, гарантируя освобождение ресурса.

Для асинхронных ресурсов используется синтаксис `using await`, который автоматически вызывает `await [Symbol.asyncDispose]()`.

## Пример

```js
class Resource {
  constructor(name) {
    this.name = name
    this.isOpen = true
  }
  read() {
    if (this.isOpen) {
      return `Ресурс ${this.name}`
    }
  }

  close() {
    if (this.isOpen) {
      this.isOpen = false
      console.info(`Ресурс ${this.name} освобождён`)
    }
  }
}

function openFile(name) {
  return new Resource(name)
}

function workWithResource() {
  let file
  try {
    file = openFile("file1.txt")
    console.log(file.read())
  } finally {
    file?.close()
  }
}

workWithResource()
```