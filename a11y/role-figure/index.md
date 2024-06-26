---
title: "`figure`"
description: "Роль для иллюстраций, схем и другого автономного содержимого."
authors:
  - tatianafokina
contributors:
  - skorobaeus
related:
  - a11y/aria-roles
  - a11y/role-caption
  - html/figure-figcaption
tags:
  - doka
---

## Кратко

[Роль структуры документа](/a11y/aria-roles/#roli-struktury-dokumenta) из [WAI-ARIA](/a11y/aria-intro/#specifikaciya). Нужна для элемента, который содержит какое-то автономное содержимое с общей подписью к нему. Например, одну или несколько картинок, иллюстрации, графики, видео, аудио или пример кода. Ещё в `figure` можно добавлять интерактивные элементы вроде ссылок.

Для [скринридеров](/a11y/screenreaders/) и других вспомогательных технологий `figure` означает, что это дополнительное содержимое страницы. Оно не важно для понимания контента, так что его можно удалить или пропустить без потери смысла текста.

Роль `figure` уже есть у [тега `<figure>`](/html/figure-figcaption/) по умолчанию.

## Пример

Контейнер с ролью `figure`, двумя картинками и общей подписью к ним:

```html
<div
  role="figure"
  aria-labelledby="label"
>
  <img src="dog-1.png" alt="">
  <img src="dog-2.png" alt="">
  <span id="label">
    На первой картинке чихуахуа, на второй —
    карликовый пинчер.
  </span>
</div>
```

<iframe title="Группа картинок с общей подписью" src="demos/figure-with-caption/" height="480"></iframe>

Скринридеры прочитают код демки примерно так: «Фигура, на первой картинке чихуахуа, на второй — карликовый пинчер».

А вот пример контейнера с `figure` без общей подписи, но с отдельными подписями к картинкам:

```html
<div role="figure">
  <img src="dog-1.png" alt="Чихуахуа">
  <img src="dog-2.png" alt="Карликовый пинчер">
</div>
```

<iframe title="Группа картинок без общей подписи" src="demos/figure-without-caption/" height="470"></iframe>

На этот раз скринридеры расскажут о демке так: «Фигура. Графика, чихуахуа. Графика, карликовый пинчер».

## Как пишется

Задайте любому тегу `role="figure"`. Лучше всего подойдут семантически нейтральные [`<div>`](/html/div/) или [`<span>`](/html/span/). В большинстве случаев используйте `<figure>`. Явная роль пригодится, когда не можете использовать HTML-тег или хотите повысить обратную совместимость со старыми браузерами и вспомогательными технологиями.

У контейнера с ролью `figure` может быть подпись или дополнительное описание. Если используете возможности HTML, вложите внутрь `<figure>` другой тег `<figcaption>` с текстом. Это создаст связь между контейнером и подписью, о которой узнают все современные вспомогательные технологии. Для кастомного элемента `figure` нужно приложить больше усилий.

<aside>

🧬 Описания и подписи к картинкам внутри `figure` должны быть уникальными и не повторять текст, который уже есть на странице. Подробнее об этом узнаете из статьи «[Как описывать картинки](/a11y/how-to-describe-pictures/)».

</aside>

Есть два способа добавить общую подпись к кастомному `figure`:

- [`aria-label`](/a11y/aria-label/) для подписи, о которой знают только скринридеры;
- [`aria-labelledby`](/a11y/aria-labelledby/) для подписи, которую видят и слышат все.

Общая подпись с `aria-label`:

```html
<div
  role="figure"
  aria-label="На первой картинке чихуахуа, на второй —
  карликовый пинчер"
>
  <img src="dog-1.png" alt="">
  <img src="dog-2.png" alt="">
</div>
```

Общая подпись с `aria-labelledby`:

```html
<div role="figure" aria-labelledby="label">
  <span id="label">
    На первой картинке чихуахуа, на второй —
    карликовый пинчер.
  </span>
  <img src="dog-1.png" alt="">
  <img src="dog-2.png" alt="">
</div>
```

Дополнительное расширенное описание добавляют к `figure` с помощью других ARIA-атрибутов:

- [`aria-description`](/a11y/aria-description/) для описания, которое доступно только скринридерам;
- [`aria-describedby`](/a11y/aria-describedby/) для видимого всем расширенного описания;
- [`aria-details`](/a11y/aria-details/) для ссылки на другую страницу с расширенным описанием.

Общее описание для картинок с `aria-description`:

```html
<div
  role="figure"
  aria-description="Хотя внешне чихуахуа похожи на
  карликовых пинчеров, это абсолютно разные породы. Они
  были выведены в разных странах независимо друг от друга."
>
  <img src="dog-1.png" alt="Чихуахуа">
  <img src="dog-2.png" alt="Карликовый пинчер">
</div>
```

Дополнительное описание с `aria-describedby`:

```html
<div
  role="figure"
  aria-describedby="description"
>
  <img src="dog-1.png" alt="Чихуахуа">
  <img src="dog-2.png" alt="Карликовый пинчер">
  <span id="description">
    Хотя внешне чихуахуа похожи на карликовых пинчеров,
    это абсолютно разные породы. Они были выведены в разных
    странах независимо друг от друга.
  </span>
</div>
```

Общее описание с `aria-details`:

```html
<div
  role="figure"
  aria-labelledby="label"
  aria-details="description"
>
  <span id="label">
    На первой картинке чихуахуа, на второй —
    карликовый пинчер.
  </span>
  <span>
    <a
      src="…"
      lang="en"
      id="description"
    >
      Подробный разбор отличий пород
    </a>
  </span>
  <img src="dog-1.png" alt="">
  <img src="dog-2.png" alt="">
</div>
```

Скринридеры никак не расскажут о `figure` и его содержимом, если оставить его без подписи или описания.  Это похоже на поведение [тега `<img>`](/html/img/) с пустым атрибутом `alt`.

<iframe title="Группа картинок без подписей и описания" src="demos/just-figure/" height="470"></iframe>

Для элементов с `figure` можно использовать и другие [глобальные ARIA-атрибуты](/a11y/aria-attrs/#globalnye-atributy).

### Поддержка для всех-всех-всех

Не все браузеры и скринридеры правильно рассказывают о теге `<figure>`, особенно когда в него вложены картинки с пустыми `alt`. Лучше всего с этим справляется скринридер JAWS в Chrome.

Если поддерживаете большой зоопарк технологий, задайте тегу явную роль `figure` и продублируйте в `aria-label` подпись из `<figcaption>`.

```html
<figure
  role="figure"
  aria-label="На первой картинке чихуахуа, на второй —
  карликовый пинчер."
>
  <!-- Содержимое элемента -->
  <figcaption>
    На первой картинке чихуахуа, на второй —
    карликовый пинчер.
  </figcaption>
</figure>
```
