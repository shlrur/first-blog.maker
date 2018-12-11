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

* [The Problem](#the-problem)
  * [jekyll-paginate](#jekyll-paginate)
* [Is The Problem Solved?](#is-the-problem-solved)
  * [jekyll-paginate-v2](#jekyll-paginate-v2)
  * [Apply To My Blog](#apply-to-my-blog)
* [References](#references)

---

# The Problem

문제를 살펴보기 전에, [pagination](https://jekyllrb.com/docs/pagination/) 이라는 기능을 알아보겠습니다.

Jekyll에서 pagination은 전체 posts 목록을 작은 목록으로 나누어 여러 페이지로 보여주는 방법을 말합니다. 예를들어, 20개의 posts를 한 페이지에 5개씩 4개의 페이지로 보여주는 것입니다. 이러한 방법은, posts가 많아 한 페이지에서 모든 posts를 보여주기 힘들 때 유용하게 사용할 수 있습니다.

해당 템플릿의 문제점은, pagination 기능을 사용하기 힘든 구조라는 점입니다. 저게 원하는 기능에 문제가 되는 부분을 살펴보겠습니다.

```md
<!-- _featured_tags/develov-javascript.md -->
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
{% raw %}<!-- _layouts/tag-blog.html -->
---
layout: base
---

{% assign posts = site.tags[page.slug] | where: "categories", page.category %}

{% for post in posts %}
  {% include post.html post=post link_title=true excerpt=true %}
{% endfor %}

{% include pagination.html %}{% endraw %}
```

[Aiden 님의 블로그](https://isme2n.github.io/)는 **featured_category** 와 **featured_tags** 로 모든 post를 분류합니다. Category로 크게 분류하고, 다시 tags로 소분류를 합니다. 위에서 첫 번째 코드는 develog category 내에서 javascript tag로 분류하는 post의 list를 그리는 markdown 파일입니다. 해당 파일은 **tag-blog**라는 layout을 사용합니다.

*_layouts/tag-blog.html*는 모든 post에서, category와 tag가 맞는 post를 골라낸 후 그 post들을 나열해서 간단히 보여줍니다. 여기서 문제를 찾을 수 있었습니다. *_layouts/tag-blog.html*에는 pagenation 기능이 없습니다. 저에게는 pagination 기능이 필요했기 때문에, 그대로 쓸 수가 없었습니다. 그래서 해당 페이지에서 pagenation 기능을 적용할 방법을 찾기 시작했습니다.

## jekyll-paginate

[jekyll-paginate](https://github.com/jekyll/jekyll-paginate)는 github에서 제공하는 plugin 중에 하나로서, jekyll blog에서 pagination 기능을 사용할 수 있게 해줍니다. 하지만 이 plugin은 사용하기에 한계가 있습니다. **모든 post를 정해진 개수로만 나눌 수 있고, 더 세세하게 page를 나눌 수 없습니다.** Blog에서 menu를 2단계로 나누고 각 menu마다 구분해서 post를 보여주기 위해서는 [jekyll-paginate](https://github.com/jekyll/jekyll-paginate) 말고 더 많은 기능을 제공하는 pagination plugin이 필요하게 되었습니다.

---

# Is The Problem Solved?

## jekyll-paginate-v2

[jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2) 는 [jekyll-paginate](https://github.com/jekyll/jekyll-paginate)에 비해 더 강화된 pagination 기능을 제공하는 plugin입니다. 이 plugin은 collection, categories, tags 및 locale 등으로 구분해서 pagination을 할 수 있는 기능을 제공합니다. 제가 원하던 기능입니다.

## Apply To My Blog

일단, [Aiden 님의 블로그](https://isme2n.github.io/)는 제가 필요한 기능이 없기 때문에 그대로 사용하기에 적절하지 않았습니다. [jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2) plugin을 적용해야 하기 때문에, 제가 처음에 선택했던 [Basically Basic](https://mmistakes.github.io/jekyll-theme-basically-basic/) theme을 다시 선택하기로 했습니다.

**[Basically Basic](https://mmistakes.github.io/jekyll-theme-basically-basic/) theme에서, [Aiden 님의 블로그](https://isme2n.github.io/)의 category와 tag를 사용해서 post를 구분하는 개념을 사용하되, [jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2) plugin을 적용해서 blog를 제작하기로 결정했습니다.**

```md
<!-- tag/etc.md -->
---
layout: tag-blog
title: etc
permalink: tag/etc
pagination:
    enabled: true
    tag: etc
---
```

```django
{% raw %}<!-- _layouts/tag-blog.html -->
---
layout: page
---
{% for post in paginator.posts %}
  {% include entry.html %}
{% endfor %}

<!-- Pagination links -->
<nav class="pager">
  <ul>
    <!-- Page: {{ paginator.page }} of {{ paginator.total_pages }} -->
    {% if paginator.previous_page %}
      <li><a href="{{ paginator.previous_page_path | relative_url }}" class="previous"><span class="icon icon--arrow-right">{% include icon-arrow-left.svg %}</span> {{ site.data.theme.t.newer | default: 'Newer' }}</a></li>
    {% endif %}
    {% if paginator.next_page %}
      <li><a href="{{ paginator.next_page_path | relative_url }}" class="next">{{ site.data.theme.t.older | default: 'Older' }} <span class="icon icon--arrow-right">{% include icon-arrow-right.svg %}</span></a></li>
    {% endif %}
  </ul>
</nav>{% endraw %}
```

위의 두 코드는 제 블로그에서 pagination 기능을 구현한 page의 코드입니다.

*tag/etc.md* 파일은 tag-blog layout을 사용하며, pagination 기능을 사용하기위해 <kbd>pagination: enabled: true</kbd>로 설정했으며, **etc**라는 tag를 가지는 post만 가져온다는 것을 나타내고 있습니다.

*_layouts/tag-blog.html* 파일은 *tag/etc.md* 파일에서 [jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2) plugin을 통해 filtering 된 post를 나열합니다. <kbd>paginator.posts</kbd>는 [jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2) plugin에서 제공하는, 현재 page의 post를 배열로 가지고 있습니다. 해당 정보를 <kbd>for</kbd> 구문을 사용해서 *_includes/entry.html* 파일로 화면에 그리게 됩니다.

이렇게 해서 저의 블로그에 다음과 같은 기능을 적용하는 데 성공했습니다.

* 메뉴가 왼쪽 혹은 오른쪽에 나오며, depth 가 있는 hierarchy 구조여야 한다.
* 각 메뉴마다 pagination 기능을 사용해서 post를 구분하여 나열한다.

그런데, local 환경에서는 문제없이 블로그가 실행되는데, github에서 build 된 *[http://shlrur.github.io](http://shlrur.github.io)* 주소에서는 pagination 기능이 적용되지 않는 문제가 생겼습니다. 다 했다고 생각했는데 전혀 예상치 못한 문제를 접했습니다.

다음 편에 계속됩니다.

---

# References
* [Jekyll Pagination](https://jekyllrb.com/docs/pagination/)
* [jekyll-paginate plugin](https://github.com/jekyll/jekyll-paginate)
* [jekyll-paginate-v2 plugin](https://github.com/sverrirs/jekyll-paginate-v2)