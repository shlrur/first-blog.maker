---
layout:     post
title:      "Web에서의 Rendering"
subtitle:   "Rendering on the Web"
categories: develog
tags:       javascript
comments:   true
---

개인 프로젝트를 진행하면서 계속해서 고민하고 있는 부분이 있습니다. 바로 SAP와 SSR인데요, 현재 사이트는 SAP로 구현하고 있습니다. 하지만 사이트가 검색엔진에서 검색되어져야 한다거나, 굳히 single page로 구현하지 않아도 되는 page 등의 이유로 부분적인 server side rendering도 고려하고 있습니다.

사이트가 부분적으로 완성 된 후에는 개선사항으로 SSR을 적용 할 생각입니다. 하지만 해당 기술을 적용하기 위해서는 좀더 잘 알 필요가 있기 때문에 이번 포스트를 작성하게 되었습니다.

>이 포스트는 [Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web) 의 글을 번역한 것입니다.

---

# Contents

* [Motivation](#motivation)
* [Terminology](#terminology)
* [Server Rendering](#Server Rendering)
* [Static Rednering](#Static Rednering)
* [Server Rendering vs Static Rendering](#Server Rendering vs Static Rendering)
* [Client-Side Rendering(CSR)](#Client-Side Rendering(CSR))
* [Combining serverrendering and CSR via rehydration](#Combining serverrendering and CSR via rehydration)
  * [A Rehydration Problem: One App for the Price of Two](#A Rehydration Problem: One App for the Price of Two)
* [Streaming server rendering and Progressive Rehydration](#Streaming server rendering and Progressive Rehydration)
  * [Partial Rehydration](#Partial Rehydration)
  * [Trisomorphic Rendering](#Trisomorphic Rendering)
* [SEO Considerations](#SEO Considerations)
* [Wrapping up...](#Wrapping up...)
* [References](#references)

---

# Motivation

우리는 가끔 개발자로서, application의 전체적인 구조에 영향을 미치는 사항에 대한 결정을 해야 할 때가 있습니다. 웹 개발자에게는 **logic과 rendering을 application의 어디에서 구현할 것인지에 대한 결정**이 바로 그런 것입니다. 웹 사이트를 만드는데 수 많은 방법이 있기 때문에, 어려운 결정이라고 할 수 있습니다.

여기서 우리는 지난 몇년간 Chrome을 통해서 큰 사이트들의 방법을 알 수 있었습니다. 전반적으로, **full rehydration** 접근법을 통해서 server rendering 혹은 static rendering을 고려할 것을 권장하고 있습니다.

이러한 결정을 내릴 때 고려중인 여러 architecture를 더 잘 이해하려면 각 방식에 대해서 확실히 이해하고 일관된 용어를 사용해야 합니다. 이러한 접근방식간의 차이를 잘 이해하면, 각 방식들의 performance적인 측면에서 trade-off를 더 잘 이해할 수 있습니다.

---

# Terminology

**Rendring**
* SSR: Server-Side Rendering - Client side 혹은 범용 application을 _서버에서_ HTML로 rendering 합니다.
* CSR: Client-Side Rendering - Browser에서 DOM을 사용해서 Application을 rendering합니다.
* Rehydration: Server에서 rendering 한 HTML DOM 트리와 data를 client에서 재사용하도록 자바 스크립트 뷰를 "부팅"합니다.
* Prerendering: Build시에 client-side application을 실행해서 초기 상태를 static HTML로 저장합니다.

**Performance**
* TTFB: Time to First Byte - 링크를 클릭했을 때와 해당 링크 contents의 첫 번째 bit가 들어왔을 때 사이의 시간.
* FP: First Paint - 사용자가 첫 번째 pixel을 볼 수 있는 시간.
* FCP: First Contentful Paint - Article body같은 요청한 내용이 보이는 시간.
* TTI: Time To Interactive - Page가 interactive(event 연결 등)하게 된 시간.

---

# Server Rendering

**Server rendering은 요청에 대한 page의 전체 HTML을 server에서 생성합니다. 이 방식은 browser에 전달되기 전에 page를 모두 제작하기 떄문에, client-side에서 data를 가져오거나 템플링을 위한 추가 round-trip이 발생하지 않습니다.**

Server rendering에서는 일반적으로 First Paint(FP)와 First Contentful Paint(FCP)가 빠릅니다. Page의 logic과 rendering을 server에서 실행하면 client에서 많은 JavaScript가 필요하지 않게 되므로 빠른 Time to Interactive(TTI)를 얻을 수 있습니다. 이러한 방법들은 server rendering이 사용자의 browser에 오직 text와 link만 보낸다면 유용할 수 있습니다. 이러한 접근 방식은 넓은 범위의 장치 및 네트워크 상태에서 잘 작동하며, streaming document parsing과 같은 browser 최적화에 대해서 흥미롭게 이야기할 수 있습니다.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/rendering-on-the-web/0_server-rendering-tti.png" alt="server rendering TTI">
</figure>

Server rendering의 경우, JavaScript때문에 생기는 loading으로 인해서 page가 로딩되는 것을 기다리는 경우가 거의 없습니다. Third-party JS를 제외할 수 없는 경우에도, server rendering을 사용한다면 First-party JS cost가 줄어들기 때문에 더 많은 여유 자원을 사용할 수 있습니다. 그러나 이 접근 방식의 주요 단점은 server에서 page를 생성하는데 시간이 걸리기 때문에 종종 Time to First Byte(TTFB)가 느려질 수 있다는 점입니다.

Server rendering이 자신의 application에 충분한지 여부는 어떤 유형의 환경을 구축하느냐에 달려있습니다. Server rendering과 client rendering사이에서 어떤 방식이 올바른 application 방식이냐에 대한 오랜 논쟁이 있지만, 더 중요한 사실은 server rendering을 어떤 page에는 적용하고 어떤 page에는 적용하지 않도록 고를 수 있다는 점입니다. 일부 사이트는 hybrid rendering 기술을 적용함으로써 성공을 거두었습니다. Netflix를 예로 들면, Netflix는 비교적 정적인 page는 server rendering을 사용하고, 많은 상호작용이 많이 필요한 page에서는 JS를 prefetch하여 많은 client rendering이 필요한 page가 빨리 loading 되도록 했습니다.

많은 최신의 framework, library, 그리고 architecture들을 통해서 client와 server 모두에서 동일한 application을 rendering할 수 있습니다. 이러한 기술들을 Server rendering을 위해 사용할 수 있지만, **server와 client 모두**에서 rendering을 할 수 있는 architecture들은 매우 다른 performance 특성과 trade-off들을 가지는 자체 유형의 solution이라는 점에 유의해야 합니다.

React 사용자들은 [renderToString()](https://reactjs.org/docs/react-dom-server.html) 혹은 server rendering을 위한 [Next.js](https://nextjs.org/)와 같은 solution을 사용할 수 있습니다.

Vue 사용자들은 Vue의 (server rendering guide)[https://ssr.vuejs.org/]를 보거나 [Nuxt](https://nuxtjs.org/)를 사용할 수 있습니다.

Angular 사용자들에게는 [Universal](https://angular.io/guide/universal)이 있습니다.

대부분의 인기 있는 solution은 hydration 기법을 사용하기 때문에, solution을 선택하기 전에 사용 방법을 숙지해야 합니다.

---

# References

* [Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web)
* [User-cectric Performance Metrics](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics)