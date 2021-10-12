---
title: "[Jekyll, GitHub Page] '이 카테고리의 다른 글'에 날짜 추가"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-09-29T01:32:30+09:00
categories:
  - Blog
tags:
  - Jekyll
  - GitHub Page
  - Liquid
permalink: /blog/category-date/
---
---

지난번 포스트에서 만들었던 '이 카테고리의 다른 글'에 날짜를 추가하고 손가락 이모티콘을 사용하여 가독성을 높혔습니다.
<!--more-->


## 만들었던 '이 카테고리의 다른 글'

![result](/assets/post-images/blog-category/result.png)

날짜가 없어 불편하고, 디자인이 게시물 내용과 똑같아서 튀지 않습니다.

## 날짜 추가
span태그로 날짜를 추가해줍니다. 이번에도 Liquid를 사용하여 post.date를 파싱하였습니다. post.date를 (0, 10)으로 슬라이스하면 yyyy-mm-dd 형태로 날짜가 표기됩니다.

(다만 UTC 기준이라 한국 기준 날짜와 차이가 있을 수도 있습니다.)

{% raw %}
```html
<span class="date__text">{{ post.date | slice: 0, 10}}</span></a>
```
{% endraw %}

날짜를 따로 span로 추가한 이유는 제목과 날짜에 각각 다른 글자 크기와 색깔을 사용할 것이기 때문입니다. 이를 위해 li 태그를 div 태그로 바꿔주고 data__item 클래스를 추가해주었습니다.

{% raw %}
```html
<div class="{{ include.type | default: 'list' }}__item date__item">
  ~~~
</div>
```
{% endraw %}

## 이모티콘 추가

li 태글 div 태그로 바꾸었기 때문에 제목 앞에 동그라미가 생기지 않습니다. 대신, 이모티콘을 추가해서 더 눈에 띄도록 해보았습니다.

추가로 현재 보고있는 포스트는 손가락이 반대방향을 가르키도록 하였습니다. 이때도 Liquid를 이용해 post.id 와 page.id를 비교하면 됩니다.

{% raw %}
```html
{% if post.id %}
  {% assign title = post.title | markdownify | remove: "<p>" | remove: "</p>" %}
{% else %}
  {% assign title = post.title %}
{% endif %}

<div class="{{ include.type | default: 'list' }}__item date__item">
  {% if post.id == page.id %}
    <a href="{{ post.url | relative_url }}" rel="permalink">👈 {{ title }} <span class="date__text">{{ post.date | slice: 0, 10}}</span></a>
  {% else %}
    <a href="{{ post.url | relative_url }}" rel="permalink">👉 {{ title }} <span class="date__text">{{ post.date | slice: 0, 10}}</span></a>
  {% endif %}
</div>
```
{% endraw %}

위는 수정된 archive-shorts.html 의 전체 코드 입니다.

{% include ad-contents.html %}

## SCSS 수정
SCSS를 수정해 제목과 날짜에 각각 다른 폰트 크기와 색상이 적용되도록 해줍니다.

**_archive.scss**
```scss
.date__item {
  font-size: $type-size-6;
  margin-left: 30px;
  text-indent: -30px;
  .date__text {
    font-weight: 500;
    color: gray;
    font-size: $type-size-8;
    white-space:nowrap;
  }
}
```

margin-left: 30px; text-indent: -30px; 이 코드는 각각의 리스트를 내어쓰기 해줍니다.

white-space:nowrap; 이 코드는 줄바꿈이 일어나서 날짜가 잘리는 것을 방지해줍니다.

## 결과

![result](/assets/post-images/blog-category/date.png)

좀 더 그럴듯한 '이 카테고리의 다른 글' 이 만들어졌습니다.

## Reference
* [[HTML/CSS] 들여쓰기 및 내어쓰기 방법](https://angelplayer.tistory.com/130){:target="_blank"}
* [CSS 줄바꿈 금지 - white-space:nowrap;](http://superkts.pe.kr/helper/view.php?seq=V&seq=390){:target="_blank"}
