---
title: "Поверхностное и глубокое копирование"
description: "При копировании объектов и массивов в JavaScript, данные копируются только на один уровень вглубь."
authors:
  - nlopin
related:
  - js/spread
  - js/ref-type-vs-value-type
  - js/objects-objects-everywhere
tags:
  - article
---

## Кратко

При копировании объектов или массивов JavaScript копирует данные только на один уровень вглубь. Этот тип копирования называется _поверхностным_ (shallow).

Если необходимо полностью скопировать сложную структуру данных, например, массив с объектами, то нужно делать _глубокое_ (deep) или полное копирование данных. JavaScript включает функцию [`structuredClone()`](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone), которая позволяет выполнять глубокое копирование сложных структур данных.

## Как понять

### Проблема поверхностного копирования

Поверхностное копирование работает быстро и в большинстве случаев его достаточно. Проблемы появляются, когда приходится копировать вложенные структуры:

```js
const itemsInCart = [
  { product: 'Носки', quantity: 3 },
  { product: 'Штаны', quantity: 1 },
  { product: 'Кепка', quantity: 1 },
]

const clonedCart = [...itemsInCart]
```

Если изменять элементы этой структуры после копирования, то эти изменения будут также видны в исходной структуре:

```js
clonedCart[1].quantity = 5

console.log(clonedCart)
// [
//    { product: 'Носки', quantity: 3 },
//    { product: 'Штаны', quantity: 5 },
//    { product: 'Кепка', quantity: 1 },
// ]

console.log(itemsInCart)
// [
//    { product: 'Носки', quantity: 3 },
//    { product: 'Штаны', quantity: 5 },
//    { product: 'Кепка', quantity: 1 },
// ]
```

Непримитивные типы данных, такие как массивы и объекты, хранятся [по ссылке](/js/ref-type-vs-value-type/#ssylochnye-tipy-dannyh). Так как копирование происходит только на один уровень вглубь, то при копировании массива происходит копирование _ссылок на старые объекты_ в новый массив.

В итоге получается картина, когда разные массивы ссылаются на одни и те же объекты в памяти:

```js
console.log(itemsInCart[1] === clonedCart[1])
// true
```

![Результат поверхностного копирования массива](images/shallow.png)

### Как получить глубокую копию

В JavaScript есть функция [structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone) для глубокого копирования массивов или объектов. На странице с документацией можно проверить доступность этой функции для разных версий: например, она доступна в NodeJS, начиная с версии 17.0.0.

```js
const deep = structuredClone(itemsInCart)
console.log(itemsInCart[1] === deep[1])
// false
```

Существуют и другие способы сделать глубокое копирование. Можно написать функцию глубокого копирования вручную. Скорее всего ваша функция будет [рекурсивной](/js/recursion/), и она будет работать только для конкретных данных — написать универсальную функцию не так-то просто.

Проще воспользоваться готовыми решениями, например, методом [`.cloneDeep()`](https://lodash.com/docs/4.17.15#cloneDeep) из библиотеки _Lodash_. Он надёжен и используется в десятках тысяч проектов каждый день. Кстати, изучить его реализацию можно на [GitHub](https://github.com/lodash/lodash/blob/4.17.15/lodash.js#L2620).

```js
import cloneDeep from 'lodash.clonedeep'

const deep = cloneDeep(itemsInCart)
console.log(itemsInCart[1] === deep[1])
// false
```

Ещё один способ глубокого копирования звучит достаточно глупо — нужно сериализовать копируемый объект в JSON и тут же распарсить его. В результате появится полная копия объекта:

```js
const deep = JSON.parse(JSON.stringify(itemsInCart))
console.log(itemsInCart[1] === deep[1])
// false
```

![Результат глубокого копирования массива](images/deep.png)

У этого метода есть ограничение — копируемые данные должны быть сериализуемы.

Вот примеры несериализуемых данных: примитив undefined, функция, [symbol](/js/symbol/) - при вызове JSON.stringify получаем undefined

Массивы и объекты - сериализуемы. Что будет если у них в качестве ключа или значения будут несериализуемые данные?

- для массивов: такие значения будут превращены в null;
- для объектов: такие значения будут опущены, а если symbol является ключом объекта, то он будет проигнорирован, даже при использовании функции replacer.

```js
const arr = [
  undefined,
  function() { console.log('aaa') },
  Symbol("foo"),
]
const copyArr = JSON.parse(JSON.stringify(arr))

console.log(copyArr)
// [null, null, null]

const obj = {
  a: undefined,
  method: () => {},
  [Symbol("foo")]: "foo",
}
const copyObj = JSON.parse(JSON.stringify(obj), function(k, v) {
  if (typeof k === 'symbol') {
    return 'символ';
  }

  return v;
})

console.log(copyObj)
// {}
```
