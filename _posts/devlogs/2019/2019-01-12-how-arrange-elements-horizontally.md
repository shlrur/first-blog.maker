---
layout:     post
title:      "[html/css] Block element를 가로로 배치하는 방법"
subtitle:   "How arrange block elements horizontally"
categories: develog
tags:       etc
comments:   true
---

이번 post에서는 float, display, flex, grid, bootstrap 를 이용한 block element를 가로로 배치하는 방법에 대해서 알아보겠습니다.

제가 처음 HTML과 CSS를 공부할 때 가장 먼저 맞닥뜨린 문제가 있었습니다. 바로 **div** element를 가로로 배치하는 것이었습다. 그때는 _block_ 혹은 _inline_ 에 대한 개념이 없을때라, **div** 같은 _block_ element를 어떻게 가로로 배치해야 하는지에 대해서 알지 못했습니다.

**이번 post에서는 HTML의 가장 기본적 요소인 _block_ 과 _inline_ 에 대해서 알아보고 float, display, flex, grid 그리고 bootstrap을 이용한 가로배치에 대해서 알아보겠습니다.**

---

# Contents

* [Block and inline](#block-and-inline)
  * [Block Level Elements]()
  * [Inline Level Elements]()
* [Solution](#solution)
  * [_site Folder](#_site-folder)
* [Conclusion](#conclusion)

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

# Arrange Elements Horizontally

여러 방법을 사용하여, block elements를 가로로 배치하는 방법에 대해서 알아보겠습니다.
비교적 예전에 사용하던 기술들 부터 아래에서 보여드리겠습니다.

## Using Float

Float property는 원래 text와 함께 image를 보여줄 때, image를 어떻게 띄워서(float) 배치할 것인지를 설정하기 위한 것이지만, 현재는 layout을 배치할 때 주로 사용됩니다.
Float property는 다음과 같은 값을 가질 수 있습니다.

* left: 해당 element의 container의 왼쪽에 띄워서 배치합니다.
* right: 해당 element의 container의 오른쪽에 띄워서 배치합니다.
* none: 해당 element가 띄워지지 않습니다. 단지 원래 있어야 할 자리에 위치합니다. default 값입니다.
* inherit: 해당 element는 자신의 부모와 같은 float값을 가집니다.

<p data-height="265" data-theme-id="light" data-slug-hash="rooLyW" data-default-tab="html,result" data-user="shlrur" data-pen-title="rooLyW" class="codepen">See the Pen <a href="https://codepen.io/shlrur/pen/rooLyW/">rooLyW</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<br>

### The clear Property

Clear property는 float property를 사용할 때 함께 사용하는 property로서, 해당 element의 왼쪽, 오른쪽 혹은 양쪽에 다른 element가 올수 있는지 없는지를 결정합니다.
Clear property는 다음과 같은 값을 가질 수 있습니다.

* none: 양쪽모두 element들이 float할 수 있습니다. default 값입니다.
* left: 해당 element의 왼쪽에 element들이 float할 수 없습니다.
* right: 해당 element의 오른쪽에 element들이 float할 수 없습니다.
* both: 양쪽모두 element들이 float할 수 없습니다.
* inherit: 해당 element는 자신의 부모와 같은 clear값을 가집니다.

## Using display: inline-block

## Using Flexbox

## Using Grid

## Using Position

## Using Bootstrap

---

# References
* [HTML Block and Inline Elements](https://www.w3schools.com/html/html_blocks.asp)

<script async src="https://static.codepen.io/assets/embed/ei.js"></script>