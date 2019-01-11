---
layout:     post
title:      "[html/css] Block element를 가로로 배치하는 방법"
subtitle:   "How arrange block elements horizontally"
categories: develog
tags:       etc
comments:   true
---

이번 post에서는 float, display, flex, grid, bootstrap 를 이용한 block element를 가로로 배치하는 방법에 대해서 알아보겠습니다.

제가 처음 HTML과 CSS를 공부할 때 가장 먼저 맞닥뜨린 문제가 있었습니다. 바로 <div> element를 가로로 배치하는 것이었습다. 그때는 _block_ 혹은 _inline_ 에 대한 개념이 없을때라, <div> 같은 _block_ element를 어떻게 가로로 배치해야 하는지에 대해서 알지 못했습니다.

**이번 post에서는 HTML의 가장 기본적 요소인 _block_ 과 _inline_ 에 대해서 알아보고 float, display, flex, grid 그리고 bootstrap을 이용한 가로배치에 대해서 알아보겠습니다.**

---

# Contents

* [Block and inline](#block-and-inline)
* [Solution](#solution)
  * [_site Folder](#_site-folder)
  * [Solutions for Using Jekyll-Paginate-V2](#solutions-for-using-jekyll-paginate-v2)
  * [Using CI](#using-ci)
  * [Solutions for Using Troublesome Task](#solutions-for-using-troublesome-task)
    * [Git submodule 생성](#git-submodule-생성)
    * [Travis에 Github repository 연결](#travis에-github-repository-연결)
    * [Token 생성](#token-생성)
    * [Config 파일 설정](#config-파일-설정)
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

### The <div> Element

<div> element는 가장 대표적인 block type element로서, container로 사용됩니다.
<div> element는 class, id 그리고 style 외에 따로 필요한 attribute는 없습니다.
CSS와 함께 사용될 때는 <div> element 내의 element 들에게 style을 적용합니다.

<p data-height="268" data-theme-id="0" data-slug-hash="cCyba" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/Tyriar/pen/cCyba'>The <samp> element</a> by Daniel Imms (<a href='http://codepen.io/Tyriar'>@Tyriar</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

## Inline Level Elements

---

# References
* [HTML Block and Inline Elements](https://www.w3schools.com/html/html_blocks.asp)

<script async="async" src="//codepen.io/assets/embed/ei.js"></script>