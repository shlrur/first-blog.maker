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

# Static Rendering

[Static rendering](https://frontarm.com/james-k-nelson/static-vs-server-rendering/)은 build시에 일어나며, 빠른 First Paint(FP), First Contentful Paint(FCP), 그리고 Time To Interactive(TTI)를 제공합니다.(client-side JS의 양이 제한되어 있다고 가정합니다) Server rendering과는 다르게, page의 HTML을 즉석에서 생성 할 필요가 없기 때문에 일관성있게 빠른 Time To First Byte(TTFB)를 얻을 수 있습니다. 일반적으로, static rendering은 각 URL에 대한 HTML 파일을 미리 생성해 놓습니다. 지금 보고 있는 Jekyll blog 역시 Static rendering을 사용합니다. Response인 HTML을 미리 생성해 놓으면, static render는 여러 CDN에 배포함으로서 edge-caching의 이점을 가져갈 수 있습니다.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/rendering-on-the-web/1_static-rendering-tti.png" alt="static rendering TTI">
</figure>

Static rendering을 위한 solution들은 모든 모양과 크기를 제공합니다. [Gatsby](https://www.gatsbyjs.org/)같은 경우는 개발자들이 자신의 application이 build 단계를 거치지 않고, 동적으로 동작하는 것 처럼 느끼게 디자인 되어 있습니다. [Jekyll](https://jekyllrb.com/)이나 [Metalsmith](https://metalsmith.io/)같은 다른 solution들은 static 한 성질을 이용해서 좀 더 tamplate-driven의 접근 방식을 제공합니다.

Static rendering의 단점 중 하나는 가능한 모든 URL에 대해 개별 HTML 파일을 생성해야 한다는 것입니다. 이는 URL이 무엇을 뜻하는지 예측하기 힘들거나 고유 페이지가 많은 site의 경우 여러 문제가 있을 수 있습니다.

React 사용자의 경우 [Gatsby](https://www.gatsbyjs.org/), [Next.js static export](https://nextjs.org/learn/excel/static-html-export/), 혹은 [Navi](https://frontarm.com/navi/)가 익숙할 수 있습니다. (이 library들을 사용해서 component를 쉽게 작성할 수 있습니다) 하지만, static rendering과 prerendering 사이의 차이를 이해하는 것이 중요합니다. Static rendering page는 client-side JS를 많이 실행하지 않아도 interactive한 반면, prerendering은 First Paint(FP) 혹은 SPA의 First Contentful Paint(FCP)를 향상시킵니다. Prerendering은 page가 interactive할 수 있도록 content를 client-side에서 생성합니다.

주어진 solution이 static rendering인지 prerendering인지 확실하지 않으면 다음의 test를 해볼 수 있습니다. _JavaScript를 disable로 한 후에 다시 web page를 load해보세요._ Static rendered page의 경우 JavaScript가 disable이라도 대부분의 기능이 사용 가능합니다. Prerendered page의 경우 link같은 기본 기능은 가능할지 몰라도 대부분의 page는 비활성 상태가 됩니다.

또 다른 유용한 test는 Chrome DevTools를 사용하여 네트워크를 느리게 하고 page가 interactive되기 전에 얼마나 많은 JavsScript를 download하는지 보는 것입니다. Prerendering의 경우 interactive를 위해 더 많은 JavaScript가 필요하며, JavaScript는 static rendering에서 사용하는 Progressive Enhancement 접근 방식보다 더 복잡한 경향이 있습니다.

---

# Server Rendering vs Static Rendering

Server rendering은 비장의 무기(silver bullet)가 아닙니다. (Server rendering만으로 모든걸 해결할 수 없다는 뜻입니다) 동적인 특성으로 인해 [상당한 compute overhead](https://medium.com/airbnb-engineering/operationalizing-node-js-for-server-side-rendering-c5ba718acfc9)가 발생할 수 있기 때문입니다. 많은 server rendering solution은 일찍 flush하지 않으며 TTFB를 지연시키거나 전송되는 data를 2배로 늘릴 수도 있습니다. (ex: client에서 JS로 인한 inlined state) React의 renderToString()은 synchronous이고 single-thread이기 때문에 느려질 수 있습니다. Server rendering을 **올바르게** 사용하기 위해서는 component caching, memory 사용량 관리, [memoization](https://en.wikipedia.org/wiki/Memoization) 기법 적용, 등등에 대한 solution을 찾거나 구축해야 합니다. 일반적으로 같은 application을 여러번 processing/rebuilding 하게 됩니다. (client에서 한번, server에서 한번...) Server rendering으로 인해 무언가를 더 빨리 보여줄 수 있다고 해서 개발자가 할 일이 줄어들지는 않습니다...ㅠ

Server rendering은 각 URL에 대한 HTML을 on-demand 방식으로 생성하지만, static rendering으로 생성된 contents를 제공하는 것 보다 느릴 수 있습니다. 발품을 좀 더 팔수있다면, server rendering + [HTML caching](https://freecontent.manning.com/caching-in-react/) 으로 server render time을 확 줄일 수 있습니다. Server rendering의 장점은 static rendering보다 좀 더 "live"한 data를 사용함으로써 요청에 대한 좀 더 완벽한 응답을 할 수 있다는 점입니다. Static rendering에 적합하지 않은 구체적인 예로, 개인화(personalization)가 필요한 page를 들 수 있습니다.

또한, Server rendering은 [PWA](https://developers.google.com/web/progressive-web-apps/)를 구축할 때 재미있는 질문을 할 수 있습니다. 전체 페이지 [service worker](https://developers.google.com/web/fundamentals/primers/service-workers/) caching을 하는게 좋을까요, 개별 content를 server rendering 하는 것이 좋을까요?

---

# Client-Side Rendering(CSR)

**Client-side rendering(CSR)은 browser에서 JavaScript를 사용해서 바로 page를 rendering하는 것을 뜻합니다. 모든 logic, data fetching, templating, 그리고 routing들이 모두 server가 아닌 client에서 처리됩니다.**

Client-side rendering은 mobile환경에서 빠르기가 힘듭니다. 만약 최소한의 작업만 수행하고, [JavaScript 작업을 최소화하며](https://mobile.twitter.com/HenrikJoreteg/status/1039744716210950144), 최대한 적은 수의 [round-trip delay time(RTT)](https://en.wikipedia.org/wiki/Round-trip_delay_time)를 유지한다면 순수한 server rendering 성능과 비슷해질 수 있습니다. Client-side rendering에서 사용하는 중요한 script와 data는 [HTTP/2 Server Push](https://www.smashingmagazine.com/2017/04/guide-http2-server-push/)나 <kbd><link rel=preload></kbd>를 통해서 더 빨리 전달될 수 있으며, 이는 parser를 통해서 더 빠르게 작동합니다. [PRPL 패턴](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)같은 패턴은 처음 및 다음에 실행 될 navigation들이 즉각적으로 일어나는 것 처럼 합니다.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/rendering-on-the-web/2_client-rendering-tti.png" alt="client rendering TTI">
</figure>

Client-side rendering의 가장 큰 단점은 application이 커짐에 따라 필요한 JavaScript의 양이 증가할 수 있다는 점입니다. 이는 새로운 JavaScript library, pollyfill, 그리고 third-party code를 추가할 때 특히 어려워지는데, 이 코드들은 각자 수행해야 할 것들을 위해서 경쟁할 수 있으며, 종종 page가 rendering되기 전에 처리해야 하는 것들도 있습니다. 그리고 대규모 JavaScript bundle이 필요한 Client-side rendering의 경우는 [적극적으로 code를 분할](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/)해야 하며, JavaScript에 대해서 lazy-load도 고려해야 합니다. (_필요한 것만 서비스하라: serve only what you need, when you need it_) Interactive가 거의 없거나 아예 없는 경우, server rendering이 더 확장 가능한 solution이 될 수 있습니다.

SPA(Single Page Application)을 제작하는 개발자가, 대부분의 page에서 사용하는 UI 핵심 부분을 구별할 수 있다면 [Application Shell caching](https://developers.google.com/web/updates/2015/11/app-shell) 기술을 적용할 수 있습니다. Service worker와 결합한다면, page를 반복해서 방문할 때 [perceived performance](https://en.wikipedia.org/wiki/Perceived_performance)를 획기적으로 개선할 수 있습니다.

---

# Combining server rendering and CSR via rehydration

Universal Rendering 혹은 간단히 "SSR"이라고 하는 접근 방식은 Client-Side Rendering과 Server Rendering간의 trade-off를 조절해서 두 방식 모두 사용합니다. Full page load 혹은 reload같은 navigation 요청은 server가 application을 HTML로 rendering하는 방식으로 처리합니다. 그리고 rendering에 사용되는 data와 JavaScript는 resulting document에 포함되어 처리됩니다. 신중하게 구현한다면, Server Rendering과 같이 빠른 First Contentful Paint(FCP)를 얻을 수 있으며, 그 후 [(re)hydration](https://docs.electrode.io/guides/general/server-side-data-hydration)이라는 테크닉을 사용해서 client에서 다시 rendering 하여 "pick up"할 수 있습니다. 이는 새로운 해결책이지만, 상당한 성능상의 단점이 있을 수 있습니다.

Rehydration을 사용하는 SSR의 가장 큰 단점은 Time To Interactive(TTI)에 상당히 부정적인 영향을 미칠 수 있다는 것입니다. First Paint(FP)가 개선되더라도 말이죠... SSR의 페이지는 종종 로딩도 되고 interactive한 것처럼 보이지만, 사실은 client-side JS가 실행되고 event handler가 붙을 때 까지 실제로 입력등에 반응할 수 없습니다. Mobile에서는 수 초에서 수 분까지 걸릴 수 있습니다.

아마 여러분들도 이런 경험이 있을겁니다. 어떤 page가 로드된 것처럼 보이지만, 일정 기간 동안 click이나 touch가 작동하지 않는 경우 말이죠. "왜 아무것도 안되지? 왜 스크롤을 할 수 없지???" 같은 경우 말이죠.

## A Rehydration Problem: One App for the Price of Two

Rehydration 문제는 가끔 JS때문에 interactive가 지연되는 것보다 더 문제가 될 수 있습니다. 최근의 SSR solution들은 일반적으로 UI의 data dependency들로부터 document로의 응답을 script tag로 직렬화 합니다. Server가 HTML을 rendering하는데 사용한 모든 data를 다시 요청 할 필요없이 client-side JavaScript가 server의 종료지점을 정확하게 "pick-up" 할 수 있도록 말이죠.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/rendering-on-the-web/3_html.png" alt="html">
</figure>

위의 사진과 같이, server는 navigation의 request에 대한 application UI의 description을 반환합니다. 이 UI를 작성하는데 필요한 data도 함께 말이죠. UI의 진정한 interactive는 bundle.js가 완벽히 load된 후에나 가능해집니다.

실제 website에서 수집된 performance 지표만 보면 SSR rehydration을 사용하지 않아야 할 것처럼 보입니다. 궁극적으로 그 이유는 UX(user experience)때문인데, user들이 쉽게 ["불쾌한 계곡"](https://en.wikipedia.org/wiki/Uncanny_valley)에 남겨질 수 있기 때문입니다.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/rendering-on-the-web/4_rehydration-tti.png" alt="rehydration-tti">
</figure>

Rehydration을 사용하는 SSR에도 희망은 있습니다. 단기적으로는, 캐시성이 높은 content에만 SSR을 사용하면 TTFB delay를 줄여서 prerendering과 유사한 결과를 얻을 수 있습니다. 점진적으로, 계속해서, 혹은 부분적으로 rehydrating을 한다면 이 기술을 미래에 더 실용적으로 만들 수 있을 것입니다.

---

# References

* [Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web)
* [User-cectric Performance Metrics](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics)
* [Static vs. Server Rendering](https://frontarm.com/james-k-nelson/static-vs-server-rendering/)
* [Gatsby](https://www.gatsbyjs.org/)
* [Jekyll](https://jekyllrb.com/)
* [Metalsmith](https://metalsmith.io/)
* [Next.js static export](https://nextjs.org/learn/excel/static-html-export/)
* [Navi](https://frontarm.com/navi/)
* [Operationalizing Node.js for Server Side Rendering](https://medium.com/airbnb-engineering/operationalizing-node-js-for-server-side-rendering-c5ba718acfc9)
* [memoization](https://en.wikipedia.org/wiki/Memoization)
* [HTML caching](https://freecontent.manning.com/caching-in-react/)
* [tight JavaScript budget](https://mobile.twitter.com/HenrikJoreteg/status/1039744716210950144)
* [RTT](https://en.wikipedia.org/wiki/Round-trip_delay_time)
* [aggressive code-splitting](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/)
* [perceived performance](https://en.wikipedia.org/wiki/Perceived_performance)
* [(re)hydration technique](https://docs.electrode.io/guides/general/server-side-data-hydration)