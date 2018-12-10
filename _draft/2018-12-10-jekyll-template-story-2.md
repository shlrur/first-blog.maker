---
layout:     post
title:      "[Jekyll] 나의 Jekyll template 제작기 - 2"
subtitle:   "looking for my Jekyll template"
categories: develog
tags:       etc
comments:   true
---

Jekyll Blog의 theme을 선택하기 위해서는, 먼저 자신의 blog에 필요한 기능이 무엇인지를 정리해야 합니다. 하지만 그 기능이 무료 theme 중에 없다면 직접 구현해야 합니다. 이번 포스팅에서는 필요한 기능들을 직접 구현한 경험에 대해서 이야기해 보려 합니다.

[이전 편]({{site.baseurl}}/develog/2018/11/24/jekyll-template-story-1/)에서는 Jekyll Blog theme 의 선택 과정에 관해서 이야기 했습니다. 처음 선택했던 [Basically Basic](https://mmistakes.github.io/jekyll-theme-basically-basic/) theme은 posts를 menu 별로 보여주는 기능이 없었기 때문에 [Aiden 님의 블로그](https://isme2n.github.io/) theme을 사용해서 다시 blog를 작성했지만, 이 블로그의 theme을 **사용하기 힘든 이유**를 발견했습니다.

---

# Contents

* [서론](#서론)
* [Jekyll](#jekyll)
* [Theme 선택](#theme-선택)
  * [시작](#시작)
  * [임시 선택](#임시-선택)
  * [더 좋은 테마가](#더-좋은-테마가)
* [References](#references)

---

# The Problem

문제를 살펴보기 전에, [pagination](https://jekyllrb.com/docs/pagination/) 이라는 기능을 알아보겠습니다.

Jekyll에서 pagination은 전체 posts 목록을 작은 목록으로 나누어 여러 페이지로 보여주는 방법을 말합니다. 예를들어, 20개의 posts를 한 페이지에 5개씩 4개의 페이지로 보여주는 것입니다. 이러한 방법은, posts가 많아 한 페이지에서 모든 posts를 보여주기 힘들 때 유용하게 사용할 수 있습니다.

해당 템플릿의 문제점은, pagination 기능을 사용하기 힘든 구조라는 점입니다. 저게 원하는 기능에 문제가 되는 부분을 살펴보겠습니다.

```md
#
---
layout: tag-blog
title: JavaScript
slug: javascript
category: devlog
menu: false
order: 1
header-img: "/img/js-logo.png"
---
```

```django
---
layout: base
---

{% assign posts = site.tags[page.slug] | where: "categories", page.category %}

{% for post in posts %}
  {% include post.html post=post link_title=true excerpt=true %}
{% endfor %}

{% include pagination.html %}
```



---

# References
* [Jekyll Pagination](https://jekyllrb.com/docs/pagination/)