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
* Rehydration: 
* Prerendering: 

**Performance**

---

# References

* [ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm)