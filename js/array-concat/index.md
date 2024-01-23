---
title: "`.concat()`"
description: "Возвращает новый массив, полученный при объединении нескольких массивов."
authors:
  - vitya-ne
related:
  - "array-to-spliced"
  - "array-push"
  - "array-splice"
tags:
  - doka
---

## Кратко

Метод `concat()` объединяет исходный массив с одним или несколькими указанными массивами или другими значениями и возвращает новый массив. Метод не меняет массивы, участвующие в объединении.

## Пример

Объединим два массива в новый массив. Обратите внимание — исходный массив не изменился:

```js
const months = ['март', 'апрель', 'май']
const summer = ['июнь', 'июль', 'август']

const favoriteMonths = months.concat(summer)

console.log(favoriteMonths)
// ['март', 'апрель', 'май', 'июнь', 'июль', 'август']
console.log(months)
// ['март', 'апрель', 'май']
```

Объединим три массива в новый массив, используя в качестве исходного пустой массив:

```js
const numbers = [2, 12, 85]
const strings = ['06', 'это', 'твой', 'номер']
const result = [].concat(numbers, strings)

console.log(result)
// [2, 12, 85, '06', 'это', 'твой', 'номер']
```

## Как пишется

`Array.concat` принимает в качестве аргументов элементы, объединяемые с элементами исходного массива в новый массив.

`Array.concat` возвращает новый массив, полученный при объединении.

Если метод без аргументов был вызван для массива, возвращается [поверхностная копия (shallow copy)](/js/shallow-or-deep-clone/) исходного массива:

```js
const numbers = [1, 2, 3, 4]

const copyNumbers = numbers.concat()

console.log(copyNumbers)
// [1, 2, 3, 4]
console.log(numbers)
// [1, 2, 3, 4]
```

В качестве аргументов могут выступать не только массивы, но и значения других типов:

```js
const numbers = [1, 2, 3]

const result = numbers.concat(4, 'five', null, {name: 'six'}, [[7]])

console.log(result)
// [1, 2, 3, 4, 'five', null, {name: 'six'}, [7]]
```

Исходным значением для метода `Array.prototype.concat` может быть не только массив, но и значения других типов (строка, число, объект). В этом случае исходное значение (`this`) преобразуется в объект и используется как аргумент объединения.

В случае попытки использовать `undefined` или `null` как исходное значение метода `Array.prototype.concat`, будет вызвано исключение `TypeError`.

<details>
  <summary>
    Подробнее о преобразовании в объект
  </summary>
   Для примитивных значений будет создан объект-обёртка соответствующего типа, например: `true` - `Boolean`, `'строка'` - `String`, `48` - `Number`.

   Так как `undefined` и `null` не имеют объектов-обёрток, то для этих значение
   -


```js
const numbers = [1, 2, 3, 4]

const copyNumbers = numbers.concat()

console.log(copyNumbers)
// [1, 2, 3, 4]
console.log(numbers)
// [1, 2, 3, 4]
```




## Как понять

Метод `concat()` обычно используется когда необходимо получить новый массив путём объединения нескольких массивов. В отличие от метода `splice()`, который может использоваться для добавления элементов к исходному массиву, метод `concat()` создаёт новый массив и не изменяет исходный.

Если аргумент содержит неодномерный массив, то этот массив будет добавлен как один элемент:

```js
const numbers = [1, 2, 3]

console.log(numbers.concat([4, [5, 6]]))
// [1, 2, 3, 4, [5, 6]]
```

Стандартное поведение метода можно изменить с использованием Symbol.isConcatSpreadable.

<details>
  <summary>
    Подробнее об использовании Symbol.isConcatSpreadable
  </summary>
  Согласно спецификации ECMAScript, для изменения поведения метода может использоваться специальное свойство Symbol.isConcatSpreadable.

  Если значением этого свойства для массива является `false`, то он будет добавлен как один элемент:

  ```js
  const numbers = [1, 2, 3]

  const otherNumbers = [4, 5, 6]
  otherNumbers[Symbol.isConcatSpreadable ] = false

  console.log(numbers.concat(otherNumbers))
  // [1, 2, 3, [4, 5, 6, [Symbol(Symbol.isConcatSpreadable)]: false ]]
  //           ------------------------------------------------------

  ```

  Если значением этого свойства для маccивоподобного объекта является `true`, то его свойства-элементы будут добавлены по отдельности:

  ```js
  const numbers = [1, 2, 3]

  const arrayLike = {
    [Symbol.isConcatSpreadable ]: true,
    '0': 4,
    '1': 5,
    '2': 6,
    length: 3
  }

  console.log(numbers.concat(arrayLike))
  // [1, 2, 3, 4, 5, 6]

  console.log(numbers.concat({''}))
  ```
</details>

## Подсказки

💡 Если один из массивов имеет незаполненные элементы, то при выполнении `concat()` возвращаемый массив тоже будет содержать незаполненные элементы:

```js
const colors = ['crimson', , 'purple']

console.log(colors.concat(['magenta']))
// ['crimson', <1 empty item>, 'purple', 'magenta']

```

💡 Если исходный массив или один из аргументов содержит объекты, то возвращаемый в результате работы метода `concat()` массив будет содержать для них ссылки, а не новые объекты. Таким образом, последующее изменение объекта будет видно в созданном массиве и наоборот:

```js
const skills = [{name: 'HTML'}, {name: 'CSS'}, {name: 'JS'}]

// добавим новый элемент
const result = skills.concat([{name: 'Python'}]);
console.log(result)
// [{name: 'HTML'}, {name: 'CSS'}, {name: 'JS'}, {name: 'Python'}]

// изменим элемент исходного массива
skills[1].level = 'C'

console.log(skills)
// [{name: 'HTML'}, {name: 'CSS', level: 'C'}, {name: 'JS'}]

// изменения также видны в созданном массиве
console.log(result)
// [
//  { name: 'HTML'},
//  { name: 'CSS', level: 'C'},
//  { name: 'JS' },
//  { name: 'Python' }
// ]
```
