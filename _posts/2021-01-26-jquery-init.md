---
title: "jQuery 설치 없이 사용하기[CDN]"
excerpt_separator: "<!--more-->"
layout: single
categories:
  - JavaScript
tags:
  - JavaScript
  - jQuery
  - CDN
permalink: /js/jquery-cdn/
---
---
여러 회사에서 제공하는 jQuery CDN을 이용하면 jQuery를 쉽고 빠르게 이용할 수 있습니다.

<!--more-->

{% include ad-contents.html %}

## CDN (Content Delivery Network)

CDN은 인터넷 컨텐츠 (html, javascript 등)를 제공하기 위해 전세계에 분산되어 있는 서버입니다.

CDN을 이용하면 클라이언트(브라우저)는 가장 빠른 서버에서 데이터를 다운로드할 수 있습니다.

또한 CDN을 이용하면 jQuery와 같이 여러 웹페이지에서 동일하게 사용되는 코드들을 재사용할 수 있습니다.

## jQuery CDN의 장점

  * 서버 트래픽 감소 (jQuery를 서버에 설치할 필요가 없다.)

  * 빠르다. (분산된 서버 사용, 중복 다운로드 최소화)

## jQuery CDN 사용하기

### 간단 사용법(Google CDN)
```
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
```
위의 html 태그를 jQuery를 사용하고자하는 웹페이지(html)에 추가해주기만 하면 됩니다. (2020/01/26 기준 최신버전 3.5.1)

{% include ad-contents.html %}

### 사용법

jQuery 공식 [다운로드](https://jquery.com/download/){:target="_blank"} 페이지에서 스크롤을 내려 **Using jQuery with a CDN** 항목을 찾습니다.

![jquery](/assets/post-images/jquerycdn0.png)

StackPath에서 제공하는 jQuery 공식 CDN이나 구글 등의 다른 회사에서 제공하는 CDN 중 원하는 것을 하나 선택합니다.

선택한 CDN 링크를 타고 들어가면 버전 별, jQuery CDN 사용 코드를 확인할 수 있습니다.

## Reference
* [Downloading jQuery](https://jquery.com/download/){:target="_blank"}
* [What is a CDN?](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/){:target="_blank"}

{% include ad-contents.html %}