---
layout:     post
title:      "[Jekyll] 나의 Jekyll template 제작기 - 1"
subtitle:   "looking for my Jekyll template"
categories: develog
tags:       etc
comments:   true
---

블로그를 시작한 계기는 내가 하는 일이나 평소 생활에 대한 log를 남겨보고 싶었기 때문입니다. 말 그대로 BLOG를 해보고 싶었습니다. 대학교&대학원 그리고 동아리에서 했던 프로젝트, 발표자료, 취미생활 등 모든 것들을 blog로 남겼다면 하는 후회가 들었습니다. 그래서 많은 것들을 놓쳤지만, 지금이라도 블로그를 시작해보려고 마음을 먹었습니다.

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

# 서론

처음엔 Tistory로 블로그를 시작하려 했습니다. 예전에 아는 선배에게서 받은 초대장이 있었기 때문입니다. 그리고 다른 일로 그 선배에게 연락해서 이런저런 이야기를 하다가, 블로그에 대한 이야기가 나왔습니다.

_"그때 주신 초대장으로 블로그를 시작하려 합니다."_

라고 했더니

_"대세는 github 블로그다. Github 블로그를 해봐라"_

그래서 [Jekyll](https://jekyllrb.com/) 블로그를 시작하게 됐습니다.

---

# Jekyll

처음 github 블로그라는 말을 들었을 때는, github에서 Tistory같은 블로그 제작 툴을 제공하는 줄 알았습니다.

Jekyll은 github에서 **공짜**로 **posting**해주는 **static website** generator입니다. 그렇기 때문에 Website를 직접 제작해야 하는 어려움이 있지만, site에 대한 file들을 github를 통해서 직접 관리할 수 있다는 장점이 있습니다.

여러 [Jekyll 테마를 제공](http://jekyllthemes.org/)하지만, 해당 테마에서 _마음에 들지 않거나 어떤 기능을 추가 혹은 제거하기 위해서_ 는 코딩이 필요합니다.

---

# Theme 선택

## 시작

먼저, 테마를 선택하기 전에 제가 필요로 하는 블로그의 요구사항은 아래와 같았습니다.

* 메뉴가 왼쪽 혹은 오른쪽에 나오며, depth 가 있는 hierarchy 구조여야 한다.
* 음...

맞습니다. 처음엔 아무것도 모르고 그저 메뉴를 계층으로 보일 수만 있으면 됐습니다. 이 생각을 가지고 [여기](http://jekyllthemes.org/) 서 테마를 검색해봤습니다.

그런데 아무리 찾아봐도 menu가 hierarchy 구조로 구성된 테마는 찾을 수 없었습니다. 모든 테마를 본 건 아니지만, 페이지를 아무리 넘겨도 그런 구조는 찾을 수가 없었습니다. 대책이 필요했습니다.

## 임시 선택

일단은 디자인만 보고 마음에 드는 테마를 찾아봤습니다. 기능은 따로 추가하면 되니까요. 그래서 찾은 jekyll 테마가 [Basically Basic](https://mmistakes.github.io/jekyll-theme-basically-basic/) 이었습니다. 해당 테마의 github 위치는 [Basically Basic Github](https://github.com/mmistakes/jekyll-theme-basically-basic) 입니다.

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/jekyll-template-story/0_basically-basic.jpg" alt="">
    <figcaption>Basically Basic Theme</figcaption>
</figure>

Basically Basic 테마의 메뉴 형태는 제가 원하던 hierarchy 구조가 아니었습니다.

```django
{% raw %}
<!-- _includes/navigation.html -->
<nav id="primary-nav" class="site-nav" itemscope itemtype="http://schema.org/SiteNavigationElement" aria-label="Main navigation">
  <ul id="menu-main-navigation" class="menu">
    <!-- Home link -->
    <li class="menu-item">
      <a href="{{ '/' | relative_url }}" itemprop="url">
        <span itemprop="name">{{ site.data.theme.t.home | default: 'Home' }}</span>
      </a>
    </li>

    <!-- site.pages links -->
    {% assign default_paths = site.pages | map: "path" %}
    {% assign page_paths = site.data.theme.navigation_pages | default: default_paths %}

    {% for path in page_paths %}
      {% assign my_page = site.pages | where: "path", path | first %}
      {% if my_page.title %}
        <li class="menu-item">
          <a href="{{ my_page.url | relative_url }}" itemprop="url">
            <span itemprop="name">{{ my_page.title | escape }}</span>
          </a>
        </li>
      {% endif %}
    {% endfor %}
  </ul>
</nav>
{% endraw %}
```

```yml
# _data/theme.yml
navigation_pages:
  - posts.md
  - recipes.md
  - about.md
  - cv.md
  - tags.md
  - categories.md
```

```yml
# _config.yml

# Collections
collections:
  recipes:
    output: true
    permalink: /:collection/:path/

# Front Matter Defaults
defaults:
  # _posts
  - scope:
      path: "_posts"
      type: posts
    values:
      layout: post
      read_time: true
  # _recipes
  - scope:
      path: "_recipes"
      type: recipes
    values:
      layout: post
      read_time: true
```

```md
<!-- recipes.md -->
---
title: Recipes
layout: collection
permalink: /recipes/
collection: recipes
entries_layout: grid
---

Sample document listing for the collection `_recipes`.
```

Basically Basic 테마는 위의 파일들을 사용해서 왼쪽의 menu들을 구성하고, menu마다 .md 파일을 넣어서 **collections**를 사용해서 구분하는 방식을 사용했습니다.

하지만 이런 구조의 단점이 있었습니다.

1. _posts 폴더에 .md(포스팅) 파일을 넣으면 포스트를 collection 별로 구분해서 메뉴에 넣을 수가 없습니다.
2. 메뉴 구조를 사용하기 위해서 collection 별로 구분해서 폴더에 글을 넣으면 _post 폴더에 글을 넣을 수가 없어서 index 화면에 _post list_ 를 보여줄 수가 없습니다.

한마디로, _post 폴더를 사용하면 menu 구조를 사용할 수가 없고, menu 구조를 사용하면 index 화면에 아무 글이 나오지 않는 문제가 있었습니다.

그래도 일단 심플한 디자인이 마음에 쏙 들었었고, **Paginate**, **Algolia Search**, **DISQUS Comments**, **Google Analytics** 등의 기능이 이미 구현되어 있었던 게 좋았습니다. 그래서, 일단 Basically Basic 테마를 선택해서 블로깅을 시작했습니다.

## 더 좋은 테마가

블로깅을 하면서도 menu를 hierarchy 구조로 구성하기 위해서 이것저것 찾아봤었습니다. 그러다가 [Aiden 님의 블로그](https://isme2n.github.io/)를 보게 됐습니다. 제가 원하던 hierarchy 메뉴구조를 찾을 수 있었습니다!!

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/images/jekyll-template-story/1_hierarchy-menu.png" alt="">
    <figcaption>What I Want</figcaption>
</figure>

이 블로그는 category와 tag를 사용해서 2-depth hierarchy 메뉴 구조를 만들었습니다.

```django
{% raw %}
<!-- _includes/nav.html -->
<span class="sr-only">Navigation:</span>
<ul>
  {% assign pages = site.pages %}

  {% for document in site.documents %}
    {% assign pages = pages | push: document %}
  {% endfor %}

  {% assign subpages = pages | where: "menu", false | sort: "order" %}
  {% assign pages = pages | where: "menu", true | sort: "order" %}
  {% assign count = 0%}
  {% for node in pages %}
  {% assign count = count | plus: 1 %}
    <li>
      <input type="checkbox" id="list-item-{{ count }}"/>
      <div  class="list-wrapper">
      <a class="sidebar-nav-item{% if page.url == node.url %} active{% endif %}" href="{{ node.url | relative_url }}">{{ node.title }}</a>
       {% if node.submenu %}<label class="folder" for="list-item-{{ count }}">▾</label>{% endif %}
    </div>
     <ul class="list-body">
       {% for subnode in subpages %}
           {% if subnode.category == node.slug %}
             <li>
               <a class="sidebar-nav-subitem{% if page.url == subnode.url %} active{% endif %}" href="{{ subnode.url | relative_url }}">{{ subnode.title }}</a>
             </li>
           {% endif %}
         {% endfor %}
     </ul>
    </li>

  {% endfor %}
</ul>
{% endraw %}
```

```markdown
<!-- _featured_categories/essay.md -->
---
layout: list
title: Essay
slug: essay
menu: true
submenu: false
order: 4
description: >
  평소 생각과 쓰고싶은 글을 씁니다.
---

```

```md
<!-- _featured_tags/essay-essay.md -->
---
layout: tag-blog
title: Essay
slug: essay
category: essay
menu: false
order: 1
header-img: "/img/essay-logo.jpg"
---

```

위의 3가지 파일이 메뉴를 만드는데 핵심적인 역할을 하는 파일입니다. **_featured_categories**와 **_featured_tags**폴더에 category와 tag 목록을 넣어놓고, 이 파일들을 사용해서 사이트 왼쪽의 menu와 submenu를 만드는 구조입니다.

이런 메뉴 구조가 마음에 들었기 때문에 비록 디자인은 마음에 들지 않았지만, 저의 jekyll 블로그 테마를 이 테마로 바꿨습니다. 하지만 다음날 바로 이 테마를 사용할 수 없는 기능적 이유를 찾았습니다.

다음편에 계속됩니다.

---

# References
* [Jekyll](https://jekyllrb.com/)
* [Jekyll Themes](http://jekyllthemes.org/)
* [Basically Basic](https://mmistakes.github.io/jekyll-theme-basically-basic/)
* [Basically Basic Github](https://github.com/mmistakes/jekyll-theme-basically-basic)
* [Aiden님 jekyll 블로그](https://isme2n.github.io/)