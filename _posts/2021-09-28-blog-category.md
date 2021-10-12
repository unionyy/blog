---
title: "[Jekyll, GitHub Page] '이 카테고리의 다른 글' 만들기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-09-28T20:45:30+09:00
categories:
  - Blog
tags:
  - Jekyll
  - GitHub Page
  - Liquid
permalink: /blog/category/
---
---

방문자들이 관련 포스트를 쉽게 찾아갈 수 있도록 '이 카테고리의 다른 글' 링크를 만들었습니다.

<!--more-->


## Lequid 문법
Jekyll 페이지의 문서에 Liquid 코드를 추가하면 빌드되면서 실행됩니다. Liquid를 이용하면 각 페이지에 맞는 링크들을 자동으로 생성할 수 있습니다.

* [Liquid](https://shopify.github.io/liquid/){:target="_blank"}

Liquid 문법은 공식 문서에서 확인할 수 있습니다.

## 관련 포스트 선정하기
Liquid를 이용해서 같은 카테고리의 포스트들 중에서 이전 포스트 2개, 현재 포스트, 이후 포스트 2개를 선정합니다. 이전 포스트가 모자랄 경우 이후 포스트로 채우고 이후 포스트가 모자랄 경우 이전 포스트로 채웁니다.

현재 포스트에는 highlight 타입을 주어서 강조되도록 하였습니다.

**this-category.html**
{% raw %}
```html
{% assign maxPosts = 5 %}
{% assign maxNext = 2 %}

{% assign catPosts = "" | split:',' %}
{% assign postCnt = 0 %}
{% assign prevCnt = 0 %}
{% assign nextCnt = 0 %}

{% for post in site.posts %}
    {% if page.categories[0] != post.categories[0] %}
        {% continue %}
    {% endif %}
    {% if post.id == page.id %}
        {% assign nextCnt = nextCnt | plus: 1 %}
        {% assign postCnt = postCnt | plus: 1 %}
        {% assign catPosts = catPosts | push: post }
    {% endif %}
    {% elsif nextCnt == 0 %}
        {% assign catPosts = catPosts | push: post %}
        {% assign postCnt = postCnt | plus: 1 %}
        {% assign prevCnt = prevCnt | plus: 1 %}
    {% else %}
        {% if nextCnt > maxNext %}
            {% if maxPosts == postCnt %}
                {% break %}
            {% endif %}
        {% endif %}
        {% assign catPosts = catPosts | push: post %}
        {% assign postCnt = postCnt | plus: 1 %}
        {% assign nextCnt = nextCnt | plus: 1 %} 
    {% endif %}
    {% if maxPosts < postCnt %}
        {% assign catPosts = catPosts | slice: 1, 5 %}
        {% assign postCnt = postCnt | minus: 1 %}
    {% endif %}
{% endfor %}

<div>
    <h3><a href="/categories#{{ page.categories[0] | slugify }}">{{ page.categories[0] }}</a> 카테고리의 다른 글</h3>
    <div>
        {% for post in catPosts %}
            {% if post.id == page.id %}
                {% include archive-shorts.html type="highlight" %}
            {% else %}
                {% include archive-shorts.html type="list" %}
            {% endif %}
        {% endfor %}
    </div>
</div>
```
{% endraw %}

{% include ad-contents.html %}

## 각 포스트의 html 형태 설정
기존의 archive-single.html 에서는 포스트의 제목만 나열하는 리스트가 없었습니다. 그래서 포스트의 제목들을 리스트로 나열할 수 있도록 archive-shorts.html을 작성해주었습니다.

**archive-shorts.html**
{% raw %}
```html
{% if post.id %}
  {% assign title = post.title | markdownify | remove: "<p>" | remove: "</p>" %}
{% else %}
  {% assign title = post.title %}
{% endif %}

<div class="{{ include.type | default: 'list' }}__item">
  <a href="{{ post.url | relative_url }}" rel="permalink"><li>{{ title }}</li></a>
</div>
```
{% endraw %}

## CSS로 기존 포스트 강조
this-category.html 에서 현재 포스트에 highlight 타입을 주었으므로 현재 포스트에 해당되는 링크는 highlight__item class에 속하게 됩니다.

_archive.scss 파일에 highlight__item 을 강조하는 코드를 추가합니다.
```css
.highlight__item {
  text-decoration: underline;
  font-weight: 700;
}
```

{% include ad-contents.html %}

## 원하는 위치에 카테고리 코드 삽입
포스트의 기본 레이아웃이 되는 single.html 레이아웃의 원하는 위치에 위에서 작성한 this-category.html 을 include 해줍니다.

{% raw %}
```
{% include this-category.html %}

{{ content }}

{% include this-category.html %}
```
{% endraw %}
저는 포스트를 읽기 전이나 읽은 후에 참고할 수 있도록 컨텐츠의 앞뒤로 하나씩 넣어주었습니다.

## 결과

![result](/assets/post-images/blog-category/result.png)

원했던대로 같은 카테고리의 이전 포스트 2개, 현재 포스트, 이후 포스트 2개의 링크가 만들어졌습니다. 그리고 현재 포스트는 강조됩니다!

## Reference
* [Liquid](https://shopify.github.io/liquid/){:target="_blank"}
