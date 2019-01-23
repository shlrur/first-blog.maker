---
layout:     post
title:      "[html/css] Block element를 가로로 배치하는 방법 - float"
subtitle:   "How arrange block elements horizontally"
categories: develog
tags:       etc
comments:   true
---

이번 post에서는 **float** 를 이용한 block element를 가로로 배치하는 방법에 대해서 알아보겠습니다.

---

# Contents

* [Using Float](#using-float)
  * [The clear Property](#the-clear-property)
  * [The clearfix Hack](#the-clearfix-hack)
  * [Layout](#layout)
* [References](#references)

---

# Using Float

Float property는 원래 text와 함께 image를 보여줄 때, image를 어떻게 띄워서(float) 배치할 것인지를 설정하기 위한 것이지만, 현재는 layout을 배치할 때 주로 사용됩니다.
Float property는 다음과 같은 값을 가질 수 있습니다.

* left: 해당 element의 container의 왼쪽에 띄워서 배치합니다.
* right: 해당 element의 container의 오른쪽에 띄워서 배치합니다.
* none: 해당 element가 띄워지지 않습니다. 단지 원래 있어야 할 자리에 위치합니다. default 값입니다.
* inherit: 해당 element는 자신의 부모와 같은 float값을 가집니다.

<p data-height="265" data-theme-id="light" data-slug-hash="rooLyW" data-default-tab="html,result" data-user="shlrur" data-pen-title="rooLyW" class="codepen">See the Pen <a href="https://codepen.io/shlrur/pen/rooLyW/">rooLyW</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<br>

## The clear Property

Clear property는 float property를 사용할 때 함께 사용하는 property로서, 해당 element의 왼쪽, 오른쪽 혹은 양쪽에 다른 element가 올 수 있는지 없는지를 결정합니다.
Clear property는 다음과 같은 값을 가질 수 있습니다.

* none: 양쪽모두 element들이 float할 수 있습니다. default 값입니다.
* left: 해당 element의 왼쪽에 element들이 float할 수 없습니다.
* right: 해당 element의 오른쪽에 element들이 float할 수 없습니다.
* both: 양쪽모두 element들이 float할 수 없습니다.
* inherit: 해당 element는 자신의 부모와 같은 clear값을 가집니다.

Clear property는 float property를 사용한 후 가장 흔하게 사용합니다.

Float에 clear를 할 때, float에 맞춰서 clear를 잘 사용해야 합니다. 무슨 말이냐면, _A element_ 를 왼쪽으로 float 시키려면, 다음 element인 _B element_ 의 왼쪽을 clear 해야 합니다. 그러면 _A element_ 는 왼쪽에 float 되고 clear 된 _B element_ 는 아래에 위치하게 될 것입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="XoLVzb" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="XoLVzb">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/XoLVzb/">
  XoLVzb</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The clearfix Hack

만약 어떤 float element가 자신을 감싸고 있는 container element보다 높이가 클때는(taller), container element의 바깥으로 _overflow_ 됩니다.

그럴때는 아래 코드와 같이 <kbd>overflow: auto;</kbd>를 사용합니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="EraXRw" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="EraXRw">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/EraXRw/">
  EraXRw</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

<kbd>overflow: auto;</kbd> clearfix는 margin과 padding을 적절히 사용한다면 잘 작동합니다.

아래의 코드는 <kbd>::after</kbd>를 사용하는 최근의 clearfix로서, 더 안정적으로 사용할 수 있습니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="RvNZwm" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="RvNZwm">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/RvNZwm/">
  RvNZwm</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## Layout

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="css,result" data-user="shlrur" data-slug-hash="jdELLw" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="jdELLw">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/jdELLw/">
  jdELLw</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

위의 코드는 **float**를 사용해서 layout을 정의한 것입니다.

header-menu의 li tag를 float를 사용해서 horizontally하게 배치하고 있습니다.
그리고 .clearfix::after 를 사용해서 <kbd>.column .menu</kbd>와 <kbd>.column .content</kbd>를 horizontally하게 배열하고 있습니다.

---

# References
* [CSS Layout - float and clear](https://www.w3schools.com/css/css_float.asp)

<script async src="https://static.codepen.io/assets/embed/ei.js"></script>