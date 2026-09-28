---
title: "`Iterator.prototype.find()`"
description: "Ищет и возвращает значение итератора определённое условием колбэк-функции"
baseline:
  - group: iterator-methods
    features:
      - javascript.builtins.Iterator.find
authors:
  - vitya-ne
related:
  - js/iterator
  - js/iterator-to-filter
  - js/iterator-take
tags:
  - doka
---

## Кратко

Метод `Iterator.prototype.find()` работает подобно методу массивов [`.find()`](/js/array-find/): среди доступных значений итератора ищется значение, для которого выполняется условие заданное в  колбэк-функции. Метод возвращает первое подходящее значение. Если ни одно из значений итератора не удовлетворяет функции проверки, возвращается `undefined`. О том, что такое итератор, можно прочитать в статье «[Итератор](/js/iterator/)».

## Пример

Представим, что у нас есть итератор по нескольким двухзначным числам:

```js
const iterator = [11, 13, 17, 19, 23, 29, 31, 37, 41].values()
```

Попробуем найти значение, для которого выполняется условие: число содержит цифру `7`.

```js
// Функция проверки условия
const hasSeven = (item) => `${item}`.includes('7')

console.log(iterator.find(hasSeven))
// 17

console.log(iterator.find(hasSeven))
// 37
```

## Как пишется

## Как понять
