---
layout:     post
title:      "[html/css] Block element를 가로로 배치하는 방법 - 시작"
subtitle:   "How arrange block elements horizontally"
categories: develog
tags:       etc
comments:   true
---

이번 post에서는 float, display, flex, grid 등을 이용한 block element를 가로로 배치하는 방법에 대해서 알아보겠습니다. Bootstrap4 와 같이 모든 속성이 정의되어 있고 element의 class만 지정해서 layout을 잡는 방법도 있지만, 이번 series에서는 순수하게 html과 css만을 사용해서 layout을 배치하는 방법에 대해서 알아보겠습니다.

제가 처음 HTML과 CSS를 공부할 때 가장 먼저 맞닥뜨린 문제가 있었습니다. 바로 **div** element를 가로로 배치하는 것이었습다. 그때는 _block_ 혹은 _inline_ 에 대한 개념이 없을때라, **div** 같은 _block_ element를 어떻게 가로로 배치해야 하는지에 대해서 알지 못했습니다.

**이번 post에서는 HTML의 가장 기본적 요소인 _block_ 과 _inline_ 에 대해서 알아보겠습니다.**

---

# Contents

* [Block and inline](#block-and-inline)
  * [Block Level Elements](#block-level-elements)
  * [Inline Level Elements](#inline-level-elements)
* [Continue](#continue)

---

# Block and Inline

HTML의 모든 element는 자신이 가지는 기본 display type이 있습니다. 대부분의 element가 가지는 기본 display type은 **block** 혹은 **inline** 입니다.

## Block Level Elements

Block-level element는 항상 새로운 line에 위치하며, 가능한 전체 폭을 사용합니다.
Block-level 에 해당하는 element는 다음과 같습니다.

```html
<address>, <article>, <aside>, <blockquote>, <canvas>, <dd>, <div>, <dl>, <dt>, <fieldset>, <figcaption>, <figure>, <footer>, <form>, <h1>-<h6>, <header>, <hr>, <li>, <main>, <nav>, <noscript>, <ol>, <output>, <p>, <pre>, <section>, <table>, <tfoot>, <ul>, <video>
```

### The **div** Element

<p data-height="265" data-theme-id="light" data-slug-hash="WLLwxJ" data-default-tab="result" data-user="shlrur" data-pen-title="WLLwxJ" class="codepen">See the Pen <a href="https://codepen.io/shlrur/pen/WLLwxJ/">WLLwxJ</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>) on <a href="https://codepen.io">CodePen</a>.</p>

**div element는 가장 대표적인 block type element**로서, container로 사용됩니다.
_div_ element는 class, id 그리고 style 외에 따로 필요한 attribute는 없습니다.
CSS와 함께 사용될 때는 _div_ element 내의 element 들에게 style을 적용합니다.

## Inline Level Elements

Inline-level element는 새로운 line에서 시작하지 않으며, 필요한 만큼의 폭만 사용합니다.
Inline-level 에 해당하는 element는 다음과 같습니다.

```html
<a>, <abbr>, <acronym>, <b>, <bdo>, <big>, <br>, <button>, <cite>, <code>, <dfn>, <em>, <i>, <img>, <input>, <kbd>, <label>, <map>, <object>, <q>, <samp>, <script>, <select>, <small>, <span>, <strong>, <sub>, <sup>, <textarea>, <time>, <tt>, <var>
```

### The **span** Element

<p data-height="265" data-theme-id="light" data-slug-hash="BvvKMO" data-default-tab="html,result" data-user="shlrur" data-pen-title="BvvKMO" class="codepen">See the Pen <a href="https://codepen.io/shlrur/pen/BvvKMO/">BvvKMO</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>) on <a href="https://codepen.io">CodePen</a>.</p>

**span element는 가장 대표적인 inline type element**로서, 주로 text의 container로 사용됩니다.
_span_ element역시 class, id 그리고 style 외에 따로 필요한 attribute는 없습니다.
CSS와 함께 사용될 때는 _span_ element 내의 text에 style을 적용합니다.

---

# Continue

다음 post 부터는 여러 방법을 사용하여 block elements를 가로로 배치하는 방법에 대해서 알아보겠습니다.
비교적 예전에 사용하던 기술부터 살펴보겠습니다.

---

# References
* [HTML Block and Inline Elements](https://www.w3schools.com/html/html_blocks.asp)

<script async src="https://static.codepen.io/assets/embed/ei.js"></script>