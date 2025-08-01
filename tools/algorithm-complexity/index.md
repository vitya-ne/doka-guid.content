---
title: "Как анализировать алгоритмы?"
description: "Разбираемся, что такое сложность алгоритмов, как посчитать сложность по времени и по памяти."
authors:
  - eshevlyakova
related:
  - tools/trivial-memory-model
  - js/arrays
  - js/for
tags:
  - article
---

## Кратко

Вычислительная сложность алгоритма описывает, как выполняемый алгоритмом объём работы зависит от размера входных данных.

<aside>

🤓 Более точный термин для вычислительной сложности алгоритма — _асимптотическая сложность_. Это значит, что оценка верна для _достаточно большого_ количества входных данных, но не обязательно верна для небольшого. Точное определение найдёте [в Википедии](https://ru.wikipedia.org/wiki/%D0%92%D1%8B%D1%87%D0%B8%D1%81%D0%BB%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C).

</aside>

Обычно у алгоритмов бывает две сложности:

- _временная сложность_ — как количество операций, которые выполняются при работе алгоритма, связано с объёмом входных данных;
- _сложность по памяти_ — как количество памяти, которое нужно алгоритму, связано с размером входных данных.

В обоих случаях оцениваем, как связаны используемые алгоритмом ресурсы (время или память) с количеством входных данных. Может показаться, что алгоритм медленнее работает и потребляет больше памяти, когда размер входных данных большой. Это не всегда так. К примеру, скорее всего время работы функции `const doNothing = (...asManyDataAsYouLike) => { }` не будет зависеть от количества переданных ей аргументов. Оценка сложности этой функции — O(1). Попробуем разобраться, что это значит.

## Способы оценки сложности (обозначения)

Есть несколько способов оценки сложности алгоритмов. Их основная идея – получить ограничение для функции, которая связывает размер входных данных и количество операций или размер памяти. Не стоит определять эту функцию точно, нам нужна именно оценка.

Теперь разберём некоторые способы оценки сложности алгоритма.

### O (О-большое)

_O_, читается как «О», «О-большое» или «биг (big) О», описывает оценку сложности сверху. То есть максимальное количество операций, которое алгоритм может выполнить в худшем случае. В скобках после О указывают функцию, которая ограничивает сложность. Например, O(n) означает, что сложность алгоритма растёт линейно. Это означает, что время выполнения алгоритма увеличивается прямо пропорционально размеру входных данных (к примеру, есть список из 10 элементов, алгоритм займёт определённое время. Но если будет 20 элементов, то алгоритм займёт в два раза больше времени). При этом как именно линейно не важно. Давайте рассмотрим несколько примеров.

Вычислительная сложность этого алгоритма — O(n). Мы обрабатываем каждый элемент один раз. Если в нашем массиве n элементов, мы выполним тело функции `reduce` n раз.

```js
  const sum = (someArray) => someArray.reduce((sum, value) => sum + value, 0)
```

Вычислительная сложность этого алгоритма тоже будет O(n). Мы обрабатываем каждый элемент два раза. Если в нашем массиве n элементов, то выполним тело функции `reduce` 2 × n раз. n раз для суммирования и n раз для произведения. Это описывает формула O(k × n) = O(n). В нашем случае коэффициент не имеет значения, так как он не зависит от размера входных данных.

```js
const sumAndProd = (someArray) => {
  const sum = (someArray) => someArray.reduce((sum, value) => sum + value, 0)
  const prod = (someArray) => someArray.reduce((prod, value) => prod * value, 1)
  return sum * prod
}
```

### Ω (омега)

_Ω_, читается как «Омега» или «Омега-большая», описывает оценку сложности снизу. То есть минимальное количество операций, которое алгоритм будет выполнять в лучшем случае. В скобках после Ω указывают функцию, которая ограничивает сложность. Например Ω(n) означает, что сложность растёт так же или быстрее, чем линейно. Например, квадратичная сложность n × n — это тоже Ω(n).

### Θ (тета)

Θ, читается как «Тета» или «Тета-большая», описывает плотную оценку алгоритма. В скобках после ϴ указывают функцию, которая ограничивает сложность как сверху, так и снизу. Рассмотрим предыдущий пример:

```js
const sumAndProd = (someArray) => {
  const sum = (someArray) => someArray.reduce((sum, value) => sum + value, 0)
  const prod = (someArray) => someArray.reduce((prod, value) => prod * value, 1)
  return sum * prod
}
```

Алгоритм выполняет 2 × n операций — n сложений и n умножений. Это значит, что количество операций сверху ограничено функцией от n или 2 × n < 3 × n, а снизу — функцией от n или 2 × n > n.

Для полноты картины приведём точные формулировки для каждого из определений.

- O — _f(n) = O(g(n))_. Есть положительное число `c`, которое, начиная с `n`, всегда выполняет условие 0 <= f(n) <= c × g(n).
- Омега — _f(n) = Ω(g(n))_. Есть положительное число `c` , которое, начиная с `n`, всегда выполняет условие 0 <= c × g(n) <= f(n).
- Тета — _f(n) = ϴ(g(n))_. Есть два положительных числа `c1` и `c2`, которые, начиная с `n`, всегда выполняют условие c1 × g(n) <= f(n) <= c2 × g(n).

Также есть алгоритмы, у которых вычислительная сложность по памяти — O(1). Их называют _алгоритмами на месте (in-place)_. Они могут использовать дополнительную память, но её размер не зависит от количества входных данных. Например, чтобы посчитать сумму всех чисел, используем переменную с их суммой. Эта переменная занимает место, но не зависит от количества складываемых чисел.

## Примеры

Ещё несколько примеров. Представим, что нужно сменить логотип, и мы выбираем дизайнера с самым большим опытом. У нас есть массив с числами, которые означают опыт в годах. Нам нужно найти максимальное число:

```js
function findMax(arr) {
  let max = arr[0]
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i]
    }
  }
  return max
}
```

Это решение имеет временную сложность O(n). Тело цикла `for` выполняется для каждого элемента массива, кроме первого. Например, переменная `max` для массива из трёх элементов обновится два раза.

Теперь представим, что у нас есть массив с опытом дизайнеров, который отсортирован по возрастанию. Мы всё так же хотим найти самого опытного из них:

```js
function findMax(arr) {
  return arr[arr.length - 1]
}
```

Этот код имеет временную сложность ϴ(1), потому что он выполняет только одну операцию, независимо от размера массива.

Наконец, представим, что у нас есть массив чисел, которые также обозначают опыт дизайнеров, и мы сортируем числа по возрастанию от меньшего к большему:

```js
function sortArray(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] > arr[j]) {
        let temp = arr[i]
        arr[i] = arr[j]
        arr[j] = temp
      }
    }
  }
  return arr
}
```

Временная сложность этого кода — ϴ(n ^ 2). Он выполняет n операций для каждого элемента массива — n × n. Если массив состоит из десяти элементов, то код выполнится за 100 операций.

Такие алгоритмы не стоит использовать для сортировки реальных массивов. Есть более эффективные сортировки с вычислительной сложностью n × log(n). Это минимально возможная вычислительная сложность для сортировки на основе сравнения двух элементов массива.

Теперь рассмотрим пример с алгоритмом на месте. Представим, что у нас есть две корзины с яблоками, и мы хотим поменять их местами:

```js
let basket1 = ["яблоко", "яблоко", "яблоко"]
let basket2 = ["апельсин", "апельсин", "апельсин"]

[basket1[0], basket2[0]] = [basket2[0], basket1[0]]

console.log(basket1) // ["апельсин", "яблоко", "яблоко"]
console.log(basket2) // ["яблоко", "апельсин", "апельсин"]
```

В примере меняем местами первые элементы двух массивов `basket1` и `basket2`. Используем деструктуризацию массива для временного хранения значений элементов и меняем их местами сразу в памяти.
