Основной сложностью данной задачи является определение уникальности элементов коллекции.

Попробуем для начала упростить задачу. Допустим, элементами коллекции являются примитивные значения. В этом случае наше решение может быть таким:

```js
function getUnique (array) {
  // Если это не массив, возвращаем пустой массив
  if (Array.isArray(array) === false) {
    return []
  }

  const uniqueArray = []

  array.forEach(item => {
    if (uniqueArray.includes(item)) {
      return
    }
    uniqueArray.push(item)
  })

  return uniqueArray
}
```

Сначала проверяем, что аргументом функции является массив, а затем перебираем элементы коллекции и накапливаем в новом массиве `uniqueArray` только уникальные элементы. Для проверки уникальности используем метод массивов `.includes()`.

Проверим работу нашей функции:

```js
const result = getUnique([1,2,2,1,3,0,2])
console.log(result)
// [ 1, 2, 3, 0 ]
```

Функция работает как ожидалось, но такое решение не оптимальное.

JavaScript имеет специальный тип для создания уникальных коллекций — `Set`. С его помощью решение будет проще:

```js
function getUnique (array) {
  // Если это не массив, возвращаем пустой массив
  if ( Array.isArray(array) === false) {
      return []
  }

  return [ ...new Set(array) ]
}
```

Это решение не требует перебора коллекции, так как при создании объекта `Set` в него попадут только уникальные элементы переданого массива. Затем, используя дестрктуризацию, мы преобразуем `Set` обратно в массив.

А как быть с объектами в качестве элементов коллекции? С точки зрения JavaScript следующие элементы являются уникальными:

```js
console.log(new Set([{},{},{},{}]))
// Set(4) { {}, {}, {}, {} }
```

Если определяете «равенство» объектов, можно воспользоваться [библиотекой Lodash](https://lodash.com). В библиотеку входит метод `.isEqual()`, позволяющий сравнивать объекты:

```js
function getUnique (array) {
  // Если это не массив, возвращаем пустой массив
  if ( Array.isArray(array) === false) {
    return []
  }

  const uniqueArray = []

  array.forEach(item => {
    if (uniqueArray.some(uniqueItem => _.isEqual(item, uniqueItem))) {
      return
    }
    uniqueArray.push(item)
  })

  return uniqueArray
}
```

Теперь перебираем элементы коллекции и накапливаем уникальные элементы в новом массиве `uniqueArray`, используя для поиска повторений метод массивов `.some()` и метод библиотеки Lodash `.isEqual()`.