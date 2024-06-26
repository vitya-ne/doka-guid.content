---
title: "`timer`"
description: "Отсчитываем время доступно."
authors:
  - tatianafokina
contributors:
  - skorobaeus
keywords:
  - a11y
  - ARIA
  - доступный таймер
  - доступный секундомер
  - live region
  - интерактивная область
related:
  - a11y/role-status
  - a11y/role-alert
  - a11y/aria-live
tags:
  - doka
---

## Кратко

[Роль изменяющихся областей](/a11y/aria-roles/#roli-izmenyayushchihsya-oblastey) из [WAI-ARIA](/a11y/aria-intro/#specifikaciya). Делает часть страницы изменяющейся или «живой» областью. `timer` пригодится для областей, где идёт отсчёт времени в обычном или обратном порядке.

## Пример

```html
<div role="timer">
  <span id="timer-hours">00</span>
  <span id="timer-mins">00</span>
</div>
```

## Как пишется

Добавьте к элементу атрибут `role="timer"`. Роль можно использовать для всех тегов и [ARIA-ролей](/a11y/aria-roles/).

Внутри контейнера с ролью таймера не обязательно использовать [`<time>`](/html/time/) и указывать время в машиночитаемом формате. Это могут быть обычные цифры и текст.

По умолчанию у `timer` есть свойство [`aria-live="off"`](/a11y/aria-live/). Так что [скринридеры](/a11y/screenreaders/) не расскажут об изменениях до того, как пользователь сделает на фокус на области с этой ролью.

Чтобы изменить поведение роли, используйте `aria-live` со значением `polite` и [JavaScript](/js/). Переключайте значение с `polite` на `off` или удаляйте весь атрибут через нужные интервалы времени. Например, каждые 60 минут. Так объявления не будут навязчивыми. В большинстве случаев лучше не изменять поведение `timer`. Например, постоянно рассказывать о том, сколько часов, минут и секунд осталось до Чёрной пятницы, — не самая лучшая идея. Пользователям скринридеров будет мучительно это слушать 😫 Другое дело — отсчёт времени до конца теста или на выполнение банковской операции. В этом случае пользователям важно знать, сколько у них осталось времени.

Атрибут [`aria-atomic="true"`](/a11y/aria-busy/) поможет улучшить таймер. Благодаря ему вспомогательные технологии расскажут про всё содержимое таймера, а не только про изменившиеся часы, секунды или минуты. Например, таймер отсчитывает две минуты в обратном направлении. Когда остаётся 59 секунд, без `aria-atomic` скринридер расскажет только о минутах: «Ноль минут». Так что пользователи могут подумать, что время вышло, хотя у них осталась ещё пара секунд. Чтобы объявления не были назойливыми, устанавливайте интервалы между объявлениями. Это удобно делать [функцией `setInterval()`](/js/setinterval/). Так можно добавлять и удалять `aria-live="true"` через нужные промежутки времени.

В примере элементу, в котором отсчитывается время, заданы `role="timer"` и `aria-atomic="true"`. После того как время вышло, появляется сообщение «Время вышло!». `aria-live` удаляется и добавляется через секунду после начала отсчёта, потом через минуту и затем за 10 секунд до завершения.

Подсказки про то, минуты это или секунды, добавлены с помощью псевдоэлемента [`::after`](/css/after/) и [`content`](/css/content/). Скринридеры всегда зачитывают текст из этого CSS-свойства.

```html
<h2>Оставшееся время</h2>
<div
  role="timer"
  id="timer"
  aria-atomic="true"
>
  <span id="min">02</span>
  <span id="sec">00</span>
</div>
<div id="message" aria-live="polite"></div>
```

<iframe title="Таймер с ролью timer" src="demos/basic-timer/" height="500"></iframe>

<video controls width="640">
  <source src="video/role-timer.mp4" type="video/mp4">
  <track
    label="Russian"
    kind="subtitles"
    srclang="ru"
    src="video/closed-captions.vtt"
  >
</video>

Элементам с `role="timer"` можно задавать подписи. Для видимой подойдёт атрибут [`aria-labelledby`](/a11y/aria-labelledby/), а для невидимой — [`aria-label`](/a11y/aria-label/). Только помните, что хоть на практике и можно добавлять подписи ко всем элементам, спецификация не рекомендует подписывать [`<div>`](/html/div/), [`<span>`](/html/span/) и другие неинтерактивные элементы.

## Как понять

_Изменяющаяся область_ — это область страницы, в которой содержимое обновляется из-за внешних событий или действий пользователей. Благодаря изменяющимся областям вспомогательные технологии узнают об обновлениях на странице и автоматически рассказывают о них пользователям.
