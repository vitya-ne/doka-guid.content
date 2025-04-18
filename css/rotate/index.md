---
title: "`rotate`"
description: "Как повернуть элементы на странице просто и быстро."
authors:
  - anastasiayarosh
contributors:
  - solarrust
related:
  - css/will-change
  - css/transition
  - css/transform
tags:
  - doka
---

## Кратко

Свойство `rotate` определяет вращение элемента. Раньше для поворота нужно было использовать свойство [`transform`](/css/transform/) со значением `rotate`, что не всегда было удобно. Теперь для этого есть отдельное свойство.

## Пример

Поворачиваем элемент на 30 градусов вправо:

```css
div {
  rotate: 30deg;
}
```

## Как пишется

Угол поворота должен указываться в [единицах измерения углов](/css/numeric-types/#edinicy-izmereniya-uglov).

Если указано одно значение, то элемент будет вращаться вдоль оси _z_:

```css
.element {
  rotate: 90deg;
}
```

К значению величины поворота можно добавить уточнение, по какой из трёх осей (_x_, _y_, _z_) применится значение. Эквивалентно `rotateX()`, `rotateY()`, `rotateZ()`:

```css
.element {
  rotate: x 90deg;
}
```

В таком формате можно указать угол наклона только по одной из осей. Не получится задать второе значение в этом же свойстве или ниже.

Можно указать собственный вектор и угол вращения в формате: 3 числа + угол. Аналогично функции `rotate3d()`.

```css
.element {
  rotate: 0 0 1 45deg;
}
```

Каждое из трёх чисел отвечает за соответствующую ось (_x_, _y_, _z_). 0 значит, что вращения по этой оси не будет. Всё, что больше нуля, устанавливает точку на этой оси. В итоге элемент будет повёрнут вокруг точки на пересечении всех трёх осей.

> Помните, что все оси по умолчанию выходят из верхнего левого угла элемента. Это можно изменить при помощи [`transform-origin`](/css/transform-origin/).

<iframe title="Демонстрация разных значений свойства rotate" src="demos/basic/" height="400"></iframe>

## Как понять

Отдельное свойство `rotate` пришло на замену функции [`rotate()`](https://doka.guide/css/transform-function/#funkcii-povorota). Не смотря на то, что свойство `transform` с его функциями очень мощное, часто бывало, что нужно переписывать весь трансформ со всеми его значениями, чтобы изменить только один параметр. Теперь для несложных поворотов можно использовать `rotate`. Также не нужно запоминать порядок позиционирования функции, в отличие от `transform`.

`rotate` поддерживает переходы и анимацию CSS, которые можно делать с помощью [`:hover`](/css/hover/) и [`@keyframes`](/css/keyframes/).

<iframe title="Анимация при помощи свойства rotate" src="demos/animation/" height="300"></iframe>
