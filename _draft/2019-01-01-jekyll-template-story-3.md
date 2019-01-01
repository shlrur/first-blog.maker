---
layout:     post
title:      "[Jekyll] 나의 Jekyll template 제작기 - 3(final)"
subtitle:   "looking for my Jekyll template"
categories: develog
tags:       etc
comments:   true
---

Jekyll은 github에서 제공하는 static website generator입니다. Github repository에 jekyll blog를 위한 code를 push하면, github가 static website를 만들어줍니다. 이 때에, [github에서 제공하는 dependency](https://pages.github.com/versions/)는 한정되어 있습니다. 바꿔말하면, 여기서 제공하지 않는 dependency는 사용할 수 없다는 뜻입니다.

[이전 편]({{site.baseurl}}/develog/2018/11/24/jekyll-template-story-2/)에서는 제가 원하는 요구사항에 맞춰서 blog theme을 만드는 과정을 보여드렸습니다. [Basically Basic](https://mmistakes.github.io/jekyll-theme-basically-basic/) theme에서, [Aiden 님의 블로그](https://isme2n.github.io/)의 category와 tag를 사용해서 post를 구분하는 개념을 사용하여, [jekyll-paginate-v2](https://github.com/sverrirs/jekyll-paginate-v2) plugin을 적용해서 blog를 제작하는데 성공하였습니다.

**하지만, 해당 결과물을 blog 생성을 위한 [github repository](https://github.com/shlrur/first-blog.maker)에 push 했을 때 jekyll blog가 제대로 작동하지 않았습니다.**

---

# Contents

* [The Problem](#the-problem)

---

# The Problem

---

# References
* [Github Pages Dependency Versions](https://pages.github.com/versions/)