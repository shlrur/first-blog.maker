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



# Webpack

Module이 뭔지는 이제 알겠습니다. 프로그램을 기능 덩어리로 나눈어서 구현하면, 그것을 알아서 bundling


# References

* [webpack's concepts](https://webpack.js.org/concepts/)