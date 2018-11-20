---
layout:     post
title:      "[Babel] Why using Babel"
subtitle:   "Compiler"
categories: develog
tags:       api
comments:   true
---

**Babel**은 Webpack과 함께 React에서 꼭 사용되는 third party library 중의 하나입니다.

webpack에 대해서는 [Why using webpack]({{ site.url }}{{ site.baseurl }}/develog/2018/11/14/why-using-webpack/) 에서 개념과 함께 필요한 이유에 대해서 알아보았습니다. 이번 포스트에서는 **Babel**이 필요한 이유와 몇 가지 플러그인에 대해서 알아보겠습니다.

---

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

---

# How to Use Babel

이번 포스트에서는 frontend, 더 특정하자면 react에서 babel을 사용하는 방법에 대해서 이야기 해보려고 합니다. 이 포스트를 작성하는 이유가 react를 사용한 프로젝트의 공부를 하기 위해서이기 때문입니다. 각설하고, CLI tool이 아닌 Configuration에 의한 사용법에 대해서 알아보도록 하겠습니다.

Webpack의 loader로서 Babel을 이용할때는 **File-relative** 방식의 config를 사용할 수 있습니다. Project-wide 방식도 있지만, 여기서는 File-relative 방식만을 다루도록 하겠습니다.

## Config in React

Babel을 webpack의 loader로 사용하기 위해서는, webpack과 babel 두 군데의 설정을 해줘야 합니다.

```js
// ./webpack.config.js
module.exports = {
    module: {
        entry: ['@babel/polyfill', './src/index.js'],
        rules: [
            {
                test: /\.(js|jsx)$/,
                exclude: /node_modules/,
                use: ['babel-loader']
            }
        ]
    }
};
```

위의 코드는 webpack의 설정파일인 _webpack.config.js_ 입니다. 파일의 위치는 project의 root입니다.

output, plugins등의 설정은 생략하고 loader인 _rules_과 entry만 표현했습니다.

entry에 _@babel/polyfill_ 이 있습니다. 해당 설정은 아래에서 설명하겠습니다.

그리고 rules 배열에 babel을 사용하는 설정이 있습니다.

*.js 혹은 .jsx 파일을 babel을 사용해서 변환하고, node_modules 폴더는 생략한다*

라는 뜻입니다.

```js
// ./.babelrc
{
    "presets": [
        "@babel/preset-env",
        "@babel/preset-react"
    ]
}
```

위의 코드는 babel에 대한 설정을 하는 _.babelrc_ 파일입니다. 역시 project의 root에 있습니다.

위의 코드는 간단합니다. _@babel/preset-env_ 과 _@babel/preset-react_ 를 적용한다는 뜻입니다. 그럼 각각의 preset이 어떤 역할을 하는지 알아보겠습니다.

### @babel/polyfill

ES2015+의 [새로운 기능](https://devhints.io/es6)을 사용할 수 있도록 코드를 바꿔줍니다. 

### @babel/preset-env

@babel/preset-env은 최신 코드(ES2015 혹은 이상의)를 사용할 때 코드가 실행되는 환경에서 필요한 syntax 변환을 해주는 preset입니다. 한마디로, 현재 브라우저에서 지원하지 않는 어떤 syntax가 발견됐을 때, 해당 syntax를 현재 브라우저에서 지원하는 정도의 syntax로 변환시켜줍니다.

그럼 어떤 syntax에 대한 처리를 해주는 걸까요? JavaScript는 아직 표준으로 정의되지 않는 proposal spec들이 존재합니다. 예전에는 이 spec들을 stage로 구분해서 babel-preset-stage-0, babel-preset-stage-1, babel-preset-stage-2, babel-preset-stage-3, babel-preset-stage-4를 지원했고, 목적에 맞게 설정해서 사용했었습니다. 하지만 이제는 **@babel/preset-env** 하나면 세세한 설정을 할 필요가 없습니다.

### @babel/preset-react

React를 사용하는 project를 위한 preset입니다. 해당 preset은 3가지 preset을 가지고있고, 2가지 preset은 development 옵션이 있을때 포함됩니다.

항상 포함하는 preset은 다음과 같습니다.

* _@babel/plugin-syntax-jsx_: JSX의 parsing을 가능하게 해줍니다.
* _@babel/plugin-transform-react-jsx_: JSX를 react의 함수(_React.createElement()_)를 호출하는 방식으로 전환시켜줍니다.
* _@babel/plugin-transform-react-display-name_: _createReactClass_ 와 _React.createClass_ 에 displayName을 추가시켜줍니다.

development 옵션일 때 추가되는 preset은 다음과 같습니다.

* _@babel/plugin-transform-react-jsx-self_: JSX element에 **__self** property를 추가합니다. 이는 runtime warning 을 생성하는데 사용됩니다. Development mode에서는 해당 preset을 사용해야 합니다.
* _@babel/plugin-transform-react-jsx-source_: JSX element에 **__source** property를 추가합니다. 해당 property에는 file name과 line number가 있는데, 이는 warning message를 생성하는데 도움을 줍니다.

---

# Conclusion

해당 post에서는 **babel**을 사용하는 이유에 대해서 알아보고, react를 사용하는 project에서의 babel 설정 방법과 각 설정 항목들이 맡은 역할에 대해서 간략하게 알아봤습니다. Project를 진행함에 따라서 babel의 세세한 설정이 필요한 경우가 있거나, 다른 조사가 필요한 경우 그에 대한 post를 또 작성해보도록 하겠습니다.

---

# References
* [Babel Docs](https://babeljs.io/docs/en/#babel-is-a-javascript-compiler)