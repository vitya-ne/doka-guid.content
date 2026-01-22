---
title: "`fetch()`"
description: "Модно и современно отправляем запросы на сервер."
authors:
  - windrushfarer
contributors:
  - vitya-ne
keywords:
  - фетч
  - запрос
related:
  - tools/http-protocol
  - recipes/progress
  - recipes/dragndrop-upload
tags:
  - doka
---

## Кратко

С помощью глобальной функции `fetch()` можно отправлять сетевые запросы на сервер — как получать, так и отправлять данные. Метод возвращает [промис](/js/promise/) с объектом ответа, где находится дополнительная информация (статус ответа, заголовки) и ответ на запрос.

## Пример

Выполним запрос для проверки доступа к тестовому [API](/tools/api/):

```js
fetch('https://dummyjson.com/test')
.then(response => response.json())
.then(data => console.log(data))
// { status: 'ok', method: 'GET' }
```

Выполним такой же запрос, используя [синтаксис async/await](/js/async-await/):

```js
const response = await fetch('https://dummyjson.com/test')
const data = await response.json()
console.log(data)
// { status: 'ok', method: 'GET' }
```

## Как пишется

```js
fetch(resource, options)
```

Функция `fetch()` принимает два параметра:

- `resource` — строка, содержащая [URL](/tools/url/)-адрес, по которому нужно сделать запрос или специальный Request-объект, хранящий данные запроса;
- `options` (необязательный) — объект конфигурации, в котором можно настроить метод и тело запроса, заголовки и многое другое.

Для выполнения GET-запроса к серверу по url-адресу можно использовать краткий синтаксис: `fetch(url)`.

Функция `fetch()` возвращает промис с результатом запроса.

При вызове `fetch()` может возникнуть исключение TypeError в случае:

- некорректный URL-адрес;
- объект конфигурации _options_ содержит недопустимые параметры;

## Как понять

Браузер предоставляет глобальный API для работы с запросами и ответами [HTTP](/tools/http-protocol/). Раньше для подобной работы использовался XMLHttpRequest, однако `fetch()` более гибкая и мощная альтернатива. Он понятнее и проще в использовании из-за того, что использует `Promise`.

Результатом вызова `fetch()` будет `Promise`, в котором содержится специальный объект ответа `Response`. У этого объекта есть два важных для нас поля:

- `ok` — принимает состояние `true` или `false` и сообщает об успешности запроса;
- `json` — метод, вызов которого, возвращает результат запроса в виде JSON.

В следующем примере используем `.then()` — обработчик результата, полученного от асинхронной операции. Обработчик дождётся ответа от сервера, принимает ответ, и, в данном случае, неявно вернёт ответ, обработанный методом `.json()`.

В примере функция `then` вернёт другой промис (их можно объединять). Когда отрезолвится промис `(r.json())`, который вернула функция `then`, будет вызван следующий колбэк в цепочке.

```js
fetch('http://jsonplaceholder.typicode.com/posts')
  .then((response) => response.json()
  // Получим ответ в виде массива из объектов:
  // [{...}, {...}, {...}, ...]
)
```

С помощью второго аргумента `options` можно передать настройки запроса. Например, можно изменить метод и добавить тело запроса, если мы хотим не получать, а отправлять данные. Также в запрос можно добавить заголовки в виде объекта или специального класса `Headers`.

```js
const newPost = {
  title: 'foo',
  body: 'bar',
  userId: 1,
}

fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST', // Здесь так же могут быть GET, PUT, DELETE
  // Тело запроса в JSON-формате
  body: JSON.stringify(newPost),
  headers: {
    // Добавляем необходимые заголовки
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((data) => {
    console.log(data)
    // {title: "foo", body: "bar", userId: 1, id: 101}
  }
)
```

### Cookies

По умолчанию `fetch()` запросы не включают в себя [cookies](/js/cookie/), и поэтому авторизованные запросы на сервере могут не пройти. Для этого необходимо добавить в настройку поле `credentials`:

```js
fetch('https://somesite.com/admin', {
  method: 'GET',
  // Или same-origin, если можно делать такие запросы
  // только в пределах этого домена
  credentials: 'include',
})
```

### Обработка ошибок

Любой ответ на запрос через `fetch()`, например, HTTP-код 400, 404 или 500, переводит `Promise` в состояние _fulfilled_. Промис перейдёт в состояние _rejected_ только если запрос не случился из-за сбоя сети или что-то помешало выполнению `fetch()`.

```js
// Запрос вернёт ошибку «404 Not Found»
fetch('https://jsonplaceholder.typicode.com/there-is-no-such-route').catch(
  () => {
    console.log('Error occurred!')
  }
)
// Никогда не выполнится
```

Чтобы обработать ошибку запроса, необходимо обращать внимание на поле `ok` в объекте ответа `Response`. В случае ошибки запроса оно будет равно `false`.

```js
fetch('https://jsonplaceholder.typicode.com/there-is-no-such-route')
  .then((response) => {
    // Проверяем успешность запроса и выкидываем ошибку
    if (!response.ok) {
      throw new Error('Error occurred!')
    }

    return response.json()
  })
  // Теперь попадём сюда, так как выбросили ошибку
  .catch((err) => {
    console.log(err)
  }
)
// Error: Error occurred!
```
