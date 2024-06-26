---
title: "Атрибут `accept`"
description: "При выборе прикрепляемого файла этот атрибут покажет только разрешённые типы."
authors:
  - tanugladkih
  - teplostanski
related:
  - html/input
  - html/form
  - html/label
tags:
  - doka
---

## Кратко

Атрибут `accept` добавляется тегу [`<input>`](/html/input/). Он позволяет указать, файлы какого типа пользователю нужно прикрепить.

## Пример

Рассмотрим пример формы, где пользователю предлагается прикрепить фотографию кота:

```html
<form method="post">
  <label for="cat-picture">Прикрепите фото кота</label>
  <input
    type="file"
    id="cat-picture"
    name="cat-picture"
    accept=".jpg, .jpeg, .png"
  >
  <button>Прикрепить</button>
</form>
```

Эта форма будет отображать только файлы с указанными расширениями в диалоговом окне выбора файлов.

## Как понять

Элемент [`<input>`](/html/input/) с атрибутом `accept` ограничивает типы файлов, которые можно выбрать в диалоговом окне выбора файлов. Это улучшает пользовательский опыт, делая процесс выбора файлов более удобным и менее подверженным ошибкам.

Важно понимать, что атрибут `accept` лишь показывает пользователю в открывшемся диалоговом окне файлы тех типов, которые вы указываете в значении атрибута, но не фильтрует их. Проверка типов файлов должна происходить на стороне сервера. Без этого ничто не помешает пользователю прикрепить скучный текстовый документ, хотя вы ждали фото котика.

## Как пишется

Атрибут `accept` применяется только к элементу [`<input>`](/html/input/) с типом `file`. Он может принимать следующие значения:

- `'audio/*'` — принимает звуковые файлы любого формата;
- `'video/*'` — принимает видео любого формата;
- `'image/*'` — принимает изображения любого формата;
- `'image/jpeg'` — JPEG изображения;
- `'image/png'` — PNG изображения;
- `'application/pdf'` — PDF документы;
- `'audio/mpeg'` — MP3 аудиофайлы;
- `'video/mp4'` — MP4 видеофайлы;
- строка типа MIME без расширений;
- расширения файла, перед которыми стоит точка, например, `.jpg`, `.pdf`, `.docx` и так далее.

Можно указать несколько значений, а ещё их можно сочетать:

```html
<input
  type="file"
  name="cat-pic-video"
  accept="video/*, .jpg, .jpeg, .png"
>
```

Также комбинировать MIME-типы и расширения файлов для большей гибкости:

```html
<input
  type="file"
  name="documents"
  accept="
    .pdf,
    application/msword,
    application/vnd.openxmlformats-officedocument.wordprocessingml.document
  "
>
```

Атрибут `accept` не принимает произвольные строки, не соответствующие форматам MIME или файловым расширениям. Например, значение `text` без указания расширения или MIME-типа будет недействительным.

## Демонстрация работы

Тут можно пощупать как браузер ведёт себя с расширениями для всех типов избражений, видео и определёнными расширениями документов:

<iframe title="Выбор файлов" src="demos/files" height="550"></iframe>

А тут с определёнными расширениями изображений:

<iframe title="Выбор изображений" src="demos/images" height="550"></iframe>

## Ограничения

1. **Поддержка браузерами**: некоторые старые браузеры могут не поддерживать атрибут `accept` полностью, особенно когда дело касается сложных комбинаций MIME-типов и расширений файлов. Стоит проверить поддержку атрибута в конкретных браузерах на [Can I Use](https://caniuse.com/input-file-accept).
1. **Отсутствие фильтрации на клиенте**: атрибут `accept` не предотвращает загрузку неподходящих файлов. Это только ориентир для диалогового окна выбора файлов.
1. **Требуется серверная проверка**: необходимо реализовать проверку типов файлов на стороне сервера для обеспечения безопасности и корректной работы приложения.

## Использование с другими атрибутами

Атрибут [`multiple`](/html/multiple/): позволяет пользователю выбрать несколько файлов.

```html
<input
  type="file"
  name="cat-pictures"
  accept=".jpg, .jpeg, .png"
  multiple
>
```

Атрибут [`required`](/html/required/): указывает, что поле обязательно для заполнения.

```html
<input
  type="file"
  name="cat-picture"
  accept=".jpg, .jpeg, .png"
  required
>
```

## Подсказки

💡 Что делать, если у пользователя несколько котов и он хочет показать вам всех? Поможет атрибут [`multiple`](/html/multiple/).
