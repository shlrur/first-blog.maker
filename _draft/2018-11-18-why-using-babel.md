---
layout:     post
title:      "[Babel] Why using Babel"
subtitle:   "Compiler"
categories: develog
tags:       api
comments:   true
---

React와 함께 사용되는 third party library로 **Webpack**과 **Babel**은 꼭 사용됩니다.

webpack에 대해서는 [Why using webpack]({{ site.url }}{{ site.baseurl }}/develog/2018/11/14/why-using-webpack/) 에서 개념과 함께 필요한 이유에 대해서 알아보았습니다. 이번 포스트에서는 **Babel**이 필요한 이유와 몇 가지 플러그인에 대해서 알아보겠습니다.

# Contents

---

# Why??

> Babel is a JavaScript compiler

Babel은 주로 현재 및 이전 브라우저 또는 환경에서 ECMAScript 2015+ 코드를 이전 버전과 호환되는 JavaScript 버전으로 변환하는 데 사용되는 [툴체인](https://ko.wikipedia.org/wiki/%ED%88%B4%EC%B2%B4%EC%9D%B8)입니다.

Babel이 하는일을 간단히 나열해보자면,

* syntax를 버전 또는 환경에 알맞게 바꾸고
* Target 환경에서 지원하지 않는 기능에 대한 [polyfill](https://en.wikipedia.org/wiki/Polyfill_(programming))을 수행(@babel/polyfill을 통해서)
* 소스코드 변환
* [등등](https://babeljs.io/videos.html)...

이 있습니다.

```js
// Babel Input: ES2015 arrow function
[1, 2, 3].map((n) => n + 1);

// Babel Output: ES5 equivalent
[1, 2, 3].map(function(n) {
  return n + 1;
});
```

혹시 어디서 들어본 것 같은 기능인가요? 그렇습니다. [Webpack의 Loaders]({{ site.url }}{{ site.baseurl }}/develog/2018/11/14/why-using-webpack/#loaders)가 하는일이 바로 babel이 하는 일과 같습니다. 그래서, **webpack을 bundler로 사용하는 project에서 babel을 사용해야 할 때는 webpack의 loader에서 babel을 사용하면 됩니다.**

그렇다면 babel을 사용해야 하는 project는 어떤것이 있을까요?

_TypeScript_ 를 사용하는 프로젝트가 하나의 예가 될 수 있습니다. TypeScript의 syntax는 browser가 이해할 수 없기 때문에 소스코드를 변환시켜야 합니다.

그리고, JavaScript framework중 하나인 **React**를 뼈대로 사용하는 project가 하나의 예가 될 수 있습니다. React는 **JSX**를 사용해서 script에서 **view**를 구현합니다. (**Angular**역시 script에서 view를 구현할 수 있지만 **.html** 파일로도 view를 구현할 수 있는것과 비교하면 특이한 방식이라고 볼 수 있습니다.) JSX는 ~~당연히~~ 현재 browser에서 바로 인식하지 못하기 때문에 babel의 기능이 필요하게 됩니다.