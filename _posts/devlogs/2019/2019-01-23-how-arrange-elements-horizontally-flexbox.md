---
layout:     post
title:      "[html/css] Block element를 가로로 배치하는 방법 - Flexbox"
subtitle:   "How arrange block elements horizontally"
categories: develog
tags:       etc
comments:   true
---

이번 post에서는 **Flexbox** 를 이용한 block element를 가로로 배치하는 방법에 대해서 알아보겠습니다.

---

# Contents

* [Flexbox with Other Layout Modules](#flexbox-with-other-layout-modules)
* [What is Flexbox](#what-is-flexbox)

---

# Flexbox with Other Layout Modules

일반적으로 4가지의 layout을 설정하는 방법이 있습니다.

* webpage의 section 정의를 위한 _Block_.
* text를 위한 _Inline_.
* 2차원(가로, 세로) table data를 위한 _Table_.
* element의 위치를 명확히 지정하기 위한 _Positioned_.

**Flexible Box Layout Module** 은 float나 positioning 없이 쉽게 flexible responsive layout 구조를 구현할 수 있게 합니다.

2019년 1월 현재 [Flexible Box Layout Module의 호환성](https://caniuse.com/#feat=flexbox)은 IE11에서 부분적으로 지원하는 것 외에 다른 브라우저의 최신버전에서는 모두 지원하고 있습니다.

---

# What is Flexbox

Flexbox를 자세히 살펴보기 전에, flexbox를 사용한 간단한 예제를 한번 보겠습니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="css,result" data-user="shlrur" data-slug-hash="NoPJbm" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="NoPJbm">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/NoPJbm/">
  NoPJbm</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

위의 예제는 3개의 block-level element를 _flex-container_ 라는 class를 가지는 container element로 감싸고 있습니다. Container element에는 <kbd>display: flex</kbd> 만 적용되었고, 그로 인해서 3개의 block-level element가 가로로 배치되었습니다.

이제 flex 속성의 여러 설정들을 예제를 통해서 알아보겠습니다.

---

# About Container Element

Container element는 _layout을 설정하려는 element들을 감싸고 있는 element_ 를 뜻합니다.(위의 예제에서 파란 배경을 뜻합니다.) Container element에서 flex를 이용해서 자식 element들의 layout을 설정하려면, <kbd>display: flex</kbd> 처럼 display 속성에 flex값을 사용해야 합니다.

이렇게 지정된 **flex conationer element**는 다음과 같은 속성들을 설정할 수 있습니다.

* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content

아래에서 해당 속성들에 대해 예제와 함께 자세히 알아보겠습니다.

## The **flex-direction** Property

**flex-direction** 속성은 container element내의 element들이 어느 방향으로 배치될지를 결정합니다. **column**, **column-reverse**, **row**, **row-reverse**의 값을 가집니다. 아래의 코드를 통해서 각각의 값이 어떤 작용을 하는지 알 수 있습니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="xMbeKW" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="xMbeKW">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/xMbeKW/">
  xMbeKW</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The **flex-wrap** Property

**flex-wrap** 속성은 flex item들이 wrap 되도록 혹은 안되도록 결정합니다.

아래의 예제는 12개의 flex item이 **flex-wrap** 속성에 따라서 어떻게 배치되는지 보여줍니다. **nowrap**이 기본값입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="KJwLqx" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="KJwLqx">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/KJwLqx/">
  KJwLqx</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The **flex-flow** Property

**flex-flow** 는 **flex-direction** 과 **flex-wrap** 속성을 동시에 설정할 수 있는 속성입니다.

예를들어, <kbd>flex-flow: row wrap;</kbd>는 <kbd>flex-direction: row; flex-wrap: wrap</kbd>와 같은 의미입니다.

## The **justify-content** Property

**justify-content** 속성은 flex item들을 정렬(align)할 때 사용합니다. **flex-start**가 기본값입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="MLYdvZ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="MLYdvZ">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/MLYdvZ/">
  MLYdvZ</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The **align-items** Property

**align-items** 속성은 flex item들의 세로정렬을 결정합니다. **stretch**가 기본값입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="QYwRXY" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="QYwRXY">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/QYwRXY/">
  QYwRXY</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

5개의 속성 값 중에서 **baseline**은 약간 직관적이지 않습니다. **baseline**은 flex item 내에 있는 사이즈가 다른 font의 baseline(red line)에 맞춰집니다.

## The **align-content** Property

**align-content**는 flex line을 정렬할 때 사용합니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="qgEzzY" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="qgEzzY">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/qgEzzY/">
  qgEzzY</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## Perfect Centering

그러면, 가장 흔한 style problem인 가로 세로 완전 중간에 element를 놓는 방법이 뭘까요?

정답은, flex item의 세로 정렬을 결정하는 **justify-content**와 가로 정렬을 결정하는 **align-items**의 값을 **center**로 하는 것입니다. 그러면 해당 flex item은 정확히 flex container의 중앙에 위치하게 됩니다.

---

# About Child Elements

위에서 살펴본 **Flex Container**의 바로 아래에 위치하는 element들을 **Flex(flexible) Items**라고 합니다.

**Flex Items**은 아래의 속성들을 설정할 수 있습니다.

* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self

아래에서 해당 속성들에 대해 예제와 함께 자세히 알아보겠습니다.

## The order Property

**order** 속성은 각 flex item들의 **순서**를 나타냅니다.
그러므로 value는 number여야 하며, default는 0입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="qgdYry" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="qgdYry">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/qgdYry/">
  qgdYry</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The flex-grow Property

**flex-grow** 속성은 각 flex item들의 **길이**를 나타내는 비율이라고 할 수 있습니다.
그러므로 value는 숫자여야 하고 default value는 0입니다.

<p class="codepen" data-height="306" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="LqVmyJ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="LqVmyJ">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/LqVmyJ/">
  LqVmyJ</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The flex-shrink Property

**flex-shrink** 속성은 각 flex item들이 각자 얼마나 **수축** 할 것인지를 비율로 나타냅니다.
역시 value는 숫자여야 하며 default value는 1입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="daoeRw" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="daoeRw">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/daoeRw/">
  daoeRw</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The flex-basis Property

**flex-basis** 속성은 각 flex item의 처음 길이를 지정합니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="css,result" data-user="shlrur" data-slug-hash="qgdYxJ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="qgdYxJ">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/qgdYxJ/">
  qgdYxJ</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

## The flex Property

**flex**는 **flex-grow**, **flex-shrink**, 그리고 **flex-basis** 속성을 동시에 설정할 수 있는 속성입니다.

예를들어, <kbd>flex: 0 0 200px</kbd>는 growable 하지 않고(0), shrinkable 하지 않으며(0) 초기 길이는 200pixel 이라는 뜻입니다.

## The align-self Property

**align-self** 속성은 flex item의 세로정렬을 결정합니다.

그런데, flex item들의 세로 정렬은 flex container의 **align-items**를 통해서도 한번에 설정 가능합니다.
**align-self** 속성은 flex container의 **align-items**를 override 해서 각 flex item에 가로정렬 값을 설정할 수 있는 것입니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="PVqeXZ" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="PVqeXZ">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/PVqeXZ/">
  PVqeXZ</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

---

# Responsive Website using Flexbox

그럼 지금까지 공부한 flex의 여러 속성들을 사용해서 reponsive wesite를 만들어 보겠습니다.

해당 website는 _width: 700px_ 을 기점으로,
_width < 700px_ 일 때는 mobile을 위해서 element들이 세로로 배치되고,
_width > 700px_ 일 때는 PC로 보기 편한 배치가 됩니다. Link인 a tag들은 inline-level element이기 때문에 가로로 배치되고, About Me와 TITLE HEADING 은 block-level인 div tag지만 container에서 display: flex 를 설정했기 때문에 가로로 배치되어 있습니다.

아래의 코드는 _CODEPEN_ 배너를 누른 후 큰 창에서 보시길 추천드립니다.

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="html,result" data-user="shlrur" data-slug-hash="mvJKde" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="mvJKde">
  <span>See the Pen <a href="https://codepen.io/shlrur/pen/mvJKde/">
  mvJKde</a> by Heekyum Kim (<a href="https://codepen.io/shlrur">@shlrur</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<br>

---

# References
* [CSS Flexbox](https://www.w3schools.com/css/css3_flexbox.asp)

<script async src="https://static.codepen.io/assets/embed/ei.js"></script>