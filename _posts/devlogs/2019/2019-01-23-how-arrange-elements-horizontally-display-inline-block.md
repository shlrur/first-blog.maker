---
layout:     post
title:      "[html/css] Block element를 가로로 배치하는 방법 - display: inline-block"
subtitle:   "How arrange block elements horizontally"
categories: develog
tags:       etc
comments:   true
---

이번 post에서는 **display: inline-block** 를 이용한 block element를 가로로 배치하는 방법에 대해서 알아보겠습니다.

---

# Contents

* [display Property](#display-property)
* [Using display: inline-block](#using-display-inline-block)
  * [Create Navigation Links](#create-navigation-links)
* [References](#references)

---

# display Property

먼저, CSS에서 **display**라는 속성이 가지는 값의 종류와 효과에 대해서 알아보겠습니다.

dispaly 속성은 element가 보여주는 방식을 나타냅니다.

* **inline**: Element를 <span>과 같이 inline element로 보여줍니다. 그래서 height와 width 속성은 적용되지 않습니다.
* **block**: Element를 <p>와 같이 block element로 보여줍니다. 그래서 새 line에서 시작하며, 전체 폭을 사용합니다.
* **contents**: Container를 없애고, element의 하위 element를 한 단계 위의 element로 설정합니다.
* **flex**: Element를 block-level의 flex container로 보여줍니다.
* **grid**: Element를 block-level의 grid container로 보여줍니다.
* **inline-block**: Element를 inline-level block container로 보여줍니다. Element는 inline element로 구성되지만, height와 width를 지정할 수 있습니다.
* **inline-flex**: Element를 inline-level의 flex container로 보여줍니다.
* **inline-grid**: Element를 inline-level의 grid container로 보여줍니다.
* **inline-table**: Element를 inline-level의 table로 보여줍니다.
* **list-item**: Element가 <li> element처럼 동작하게 합니다.
* **run-in**: element가 context에 따라서 block element나 inline element로 보여줍니다.
* **table**: Element가 <table> element처럼 동작하게 합니다.
* **table-caption**: Element가 <caption> element처럼 동작하게 합니다.
* **table-column-group**: Element가 <colgroup> element처럼 동작하게 합니다.
* **table-header-group**: Element가 <thead> element처럼 동작하게 합니다.
* **table-footer-group**: Element가 <tfoot> element처럼 동작하게 합니다.
* **table-row-group**: Element가 <tbody> element처럼 동작하게 합니다.
* **table-cell**: Element가 <td> element처럼 동작하게 합니다.
* **table-column**: Element가 <col> element처럼 동작하게 합니다.
* **table-row**: Element가 <tr> element처럼 동작하게 합니다.
* **none**: Element를 완전히 제거합니다.
* **initial**: display 속성을 default값으로 설정합니다. 
* **inherit**: display 속성을 부모 element의 display값으로 설정합니다.

# Using display: inline-block

<kbd>display: inline</kbd>와 비교했을 때 <kbd>display: inline-block</kbd>의 가장 큰 차이점은 **width와 height를 지정할 수 있다는 것**입니다. 또한, <kbd>display: inline-block</kbd>은 상하 margin/padding이 적용됩니다.(<kbd>display: inline</kbd>은 안됩니다)

<kbd>display: block</kbd>과 비교했을 때 가장 큰 차이점은, <kbd>display: inline-block</kbd>은 element뒤에 line이 바뀌지 않기 때문에 옆으로 쭉 나열할 수 있다는 점입니다.

아래의 예제에서는 <kbd>display: inline</kbd>, <kbd>display: inline-block</kbd> 그리고 <kbd>display: block</kbd>을 보여주면서 차이점을 비교해 볼 수 있습니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="vbEWMb" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="vbEWMb">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/vbEWMb/">
  vbEWMb</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## Create Navigation Links

<kbd>display: inline-block</kbd>을 흔하게 사용하는 경우인 navigation link를 세로가 아닌 가로로 배치하는 경우에 대해서 살펴보겠습니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="daPZxQ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="daPZxQ">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/daPZxQ/">
  daPZxQ</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

---

# References
* [CSS display Property](https://www.w3schools.com/cssref/pr_class_display.asp)
* [CSS Layout - display: inline-block](https://www.w3schools.com/css/css_inline-block.asp)

<script async src="https://static.codepen.io/assets/embed/ei.js"></script>