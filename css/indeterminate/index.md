---
title: "`:indeterminate`"
description: "Псевдокласс, который отражает неопределённое состояние чекбокса, радиокнопки или прогресс-бара."
authors:
  - marybayt
keywords:
  - selector
  - attribute
  - инпут
  - form
  - форма
  - загрузка
  - downloading
related:
  - html/input
  - html/progress
  - js/deal-with-forms
tags:
  - doka
  - placeholder
---

## Кратко

[Псевдокласс](/css/pseudoclasses/), который используется для стилизации трёх элементов: [чекбоксов](/html/input/#type), [радиокнопок](/html/input/#type) и [прогресс-баров](/html/progress/).

Пригодится в двух случаях. Во-первых,  для стилизации элементов в их исходном состоянии — при открытии формы или начале загрузки. Во-вторых, для показа пользователю незавершённости процесса выбора или загрузки.

Для чекбоксов и радиокнопок состояние `indeterminate` не получится присвоить напрямую в HTML, его можно задать **только** через JavaScript.

Прогресс-бару браузер присваивает `:indeterminate` автоматически, если не определён атрибут `value` — процент загрузки.

## Примеры

- `<input type="checkbox">`, если не все пункты вложенной группы были выделены. Показывает пользователю, что он пока взаимодействовал не со всеми опциями выбора, которые ему предложены.
- Группа `<input type="radio">` с одинаковым атрибутом `name`, но у которой ни один элемент не установлен в [`checked`](/css/checked/). Показывает пользователю, что он пропустил пункт и не выбрал свой вариант.
- [`<progress>`](/html/progress/). Показывает процесс загрузки, но не показывает его прогресс.

<iframe title="Промежуточные состояния элементов форм" src="demos/" height="400"></iframe>

## Как пишется

После селектора, который выбирает элемент прогресс-бара, чекбокса или группу радиокнопок, ставим двоеточие и пишем ключевое слово `indeterminate`, прописываем стили.

```css
input:indeterminate {
  opacity: 0.45;
}
```

## Как понять

Браузер присваивает чекбоксу или радиокнопке псевдокласс `:indeterminate`, когда они определены так через JavaScript. Мы можем использовать этот псевдокласс для стилизации нетронутого пользователем элемента формы, подчёркивая незавершённость выбора.

```javascript
const inputs = document.getElementsByTagName('input');

for (let i = 0; i < inputs.length; i++) {
  inputs[i].indeterminate = true;
}
```
