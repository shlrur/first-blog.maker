---
layout:     post
title:      "[webpack] Why using webpack"
subtitle:   "Modules"
categories: develog
tags:       api
comments:   true
---

기존의 React Boilerplate들은 여러 종류의 package를 사용하지만, 그 중에서 webpack과 babel은 거의 필수로 사용하고 있습니다. 이번 포스트에서는 필수 third party API중 Frontend에서 bundling을 위해 가장 많이 사용하는 **Webpack**이 필요한 이유에 대해서 알아보려 합니다.

> webpack is a static module bundler for modern JavaScript applications.

Webpack 공식 홈페이지의 concept을 설명하는 제일 첫 줄에 있는 문구입니다. 중요한 문장이니만큼 **module**과 **Bundle**에 대해서 알아봐야 겠습니다.


## Module

**Modular Programming**에서, 개발자들은 프로그램을 **module**이라고 부르는 별도의 기능 덩어리(_chunks of functionality_)로 분할합니다.([Low Coupling High Cohesion](https://stackoverflow.com/questions/14000762/what-does-low-in-coupling-and-high-in-cohesion-mean)) 각 module은 전체 프로그램에 비하면 작기때문에, 검증, 디버깅, 그리고 테스트 하는데 비교적 적은 노력이 필요합니다. 그리고, 잘 만들어진 module은 OOP의 요소인 abstraction과 encapsulation을 제공하기 때문에, 전체 프로그램에서 일관된 디자인과 함께 명확한 목적을 가지고 사용됩니다.

Node.js는 처음부터 modular programming을 지원했지만, 현재 web 표준인 ES6의 module은 아직 모든 브라우저에서 지원하고 있지 않습니다. 때문에, third party library를 사용해서 bundling을 해야 합니다. 현재 Web에서 **Modular JavaScript**를 지원하는 여러 third party library들이 존재하며 각각 이점과 한계를 가지고 있습니다.

## Bundle

JavaScript에서의 Modular Programming을 위해서는 일반적으로 하나의 module당 하나의 파일이 사용됩니다. 그러면 여러 파일들이 생성되는데, 이 파일들을 사용하기 위해서는 어떤 형태로든 명시를 해 줘야 합니다. 

### OLD way...

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/why_using_webpack/0_old-way.png" alt="">
    <figcaption>SpiderMonkey compiler</figcaption>
</figure>

위의 이미지는 우리가 예전에 혹은 간단한 site에서 사용하는 방식입니다.

위의 방식은 두 가지 큰 단점이 있습니다.
첫 번째는 너무 많은 global 변수들 때문에 global namespace가 엉망진창이 된다는 것입니다. 그리고 Library들이 load되는 순서가 엉켜버리지 않게 조심해야 합니다. jQuery, React, Angular 혹은 lodash같은 third party library들을 먼저 load한 후에 직접 작성한 component나 service들을 load해야 합니다.
두 번째는 각 library들의 dependency들을 볼 수 없다는 것입니다. 때문에, 특정 코드가 실행 될 때, 그 코드에서 사용하는 API의 script역시 _이미_ load되었다고 가정하는 수 밖에 없습니다. 코드들이 추가될수록, 특정 API를 쓰기위해서 context를 전환해야 할 수도있고, HTML에 script코드가 추가로 필요할수도 있습니다. 그리고 이 script들이 잘 로드되는지는 직접 실행해서 화면을 보는 수 밖에 없습니다...

### Static File Bundling

Web app들의 증가와 현대적인 frontend framework들 덕분에 JavaScript 개발 방식이 변화되기 시작하면서, static file bundling이라는 방식이 나타났습니다.

현재의 bundling은 여러 tool에 의해 frontend에서 수행되지만, backend에서 수행되던 때도 있었습니다. ASP.NET 5 이전버전의 Microsoft.AspNet.Optimization package같은 경우가 하나의 예입니다. 하지만 결국에는 JavaScript를 사용하게 되었습니다.



# Webpack

Module이 뭔지는 이제 알겠습니다. 프로그램을 기능 덩어리로 나눈어서 구현하면, 그것을 알아서 bundling


# References

* [webpack's concepts](https://webpack.js.org/concepts/)
* [Modern approach of JavaScript bundling with Webpack](https://medium.com/@andrejsabrickis/modern-approach-of-javascript-bundling-with-webpack-3b7b3e5f4e7)