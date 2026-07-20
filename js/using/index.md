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

Объявление переменной с помощью `using` связывает её с **синхронным** ресурсом. При выходе из блока, в котором переменная была объявлена, автоматически вызовется метод [Symbol.dispose](), освобождая ресурс. Для **асинхронных** ресурсов применяется синтаксис `using await` и вызывается метод `await [Symbol.asyncDispose]()`.

## Пример

Рассмотрим работу с ресурсом на примере.
Для имитации реального ресурса реализуем простой класс `FileResource`. Экземпляр класса имеет методы:
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

Напишем функцию доступа к ресурсу. Необходимо предусмотреть освобождение ресурса в случае ошибок при работе с файлом.
Поэтому используем блок [`try/catch/finally`](/js/try-catch/):

```js

function workWithFile(filename) {
  // Объявляем переменную
  let file
  try {
    file = new FileResource(filename)
    // Используем ресурс
    file.open()
    console.log(file.read())
  } finally {
    // Освобождаем ресурс
    file?.close()
  }
}

workWithFile('file1.txt')
// данные ресурса file1.txt
// ресурс file1.txt освобождён
```

Текущую реализацию функции `workWithFile` можно упростить.
Добавим метод `[Symbol.dispose]()` в класс `FileResource` и используем `using`:

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

  [Symbol.dispose]() {
    this.close()
  }
}

function workWithFile(filename) {
  using file = new FileResource(filename)
  file.open()
  console.log(file.read())
}

workWithFile('file1.txt')
```
