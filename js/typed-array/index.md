---
title: "`TypedArray`"
description: "Объект для представления бинарных данных из буфера."
authors:
  - doka-dog
  - vitya-ne
keywords:
  - typed array
  - indexed collection
  - javascript collection
  - typed array view
related:
  - js/shared-array-buffer
  - js/array-buffer
  - js/data-view
tags:
  - doka
  - placeholder
---

## Кратко

Типизированные массивы в JavaScript это специальные массивоподобные объекты предназначенные для эффективной работы с двоичными данными. Типизированные массивы предоставляют доступ для чтения и записи данных с использованием буфера — пространства в памяти, где хранятся бинарные данные.

<aside>

🧠 Про память подробнее узнаете из статьи «[Как устроена память](/tools/trivial-memory-model/)».

</aside>

Типизированные массивы `TypedArray` упрощают работу с двоичными данными, например, видео, аудио и анимациями. Их часто используют с различными API — WebGL, Canvas 2D, XMLHttpRequest2 и так далее.

Для достижения максимальной гибкости реализация `TypedArray` разделена на Буфер (Buffer) и Представление (View). Буфер — содержит данные, но не имеет формата и не предоставляет способа доступа к данным. Представление — обеспечивает доступ к данным из буфера. Можно использовать одновременно несколько представлений `TypedArray` для одного и того же буфера.


## Пример

```js
const buffer = new ArrayBuffer(4)
const view = new Int8Array(buffer)

view[0] = 1
view[2] = 2

console.log(buffer)
console.log(view)
// ArrayBuffer { [Uint8Contents]: <01 00 02 00>, byteLength: 4 }
// Int8Array(4) [ 1, 0, 2, 0 ]
```

## Как пишется

Рассмотрим способы создания типизированного массива на примере `Int8Array`, содержащего 8-ми битные целые значения (от -127 до 127):

- создаём массив с буфером длинной `length`
```js
const view = new Int8Array(length)
```

- создаём новый массив с буфером, путём копирования существующего массива `typedArray`:
```js
const view = new Int8Array(typedArray)
```

- создаём новый массив с буфером из массивоподобного объекта `arrayLike` c использованием статического метода `.from()`:
```js
const view = Int8Array.from(arrayLike)
```

- создаём новый массив используя существующий буфер `buffer`:
```js
var buffer = new ArrayBuffer(2);
var view = new Int8Array(buffer);
```


Чтобы создать типизированный массив, сначала создайте буфер с помощью [объекта `ArrayBuffer`](/js/array-buffer/) или [`SharedArrayBuffer`](/js/shared-array-buffer/), а потом его представление объектами `TypedArray` или [`DataView`](/js/data-view/).

Для создания `ArrayBuffer` используйте оператор `new`. В `TypedArray` указывают нужный размер данных, количество элементов и их начальную позицию в буфере. Для этого используют разные [числовые форматы](https://tc39.es/ecma262/multipage/indexed-collections.html#table-the-typedarray-constructors). Например, `Int8Array`, `Uint8Array`, `Float64Array`, `Uint8ClampedArray`.


## Как понять

_Коллекция в JavaScript_ — это набор данных разного типа. К примеру, в ней могут хранится массивы и объекты. Также коллекция может быть сама по себе специфической структурой данных, если в ней намешано много всего. Они бывают нескольких видов, и `TypedArray` относится к проиндексированным коллекциям.
