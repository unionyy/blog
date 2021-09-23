---
title: "[JavaScript Compressor] 자바스크립트 코드 압축하기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-03-19T17:00:30+09:00
categories:
  - JavaScript
tags:
  - JavaScript
permalink: /js/compress/
---
---
자바스크립트 코드는 서버에서 통채로 전송되어 브라우저에서 실행되므로 코드 자체의 크기를 줄이면 로딩속도를 향상시킬 수 있습니다.
<!--more-->

{% include ad-contents.html %}

## [JavaScript Compressor](http://javascriptcompressor.com/){:target="_blank"}
자바스크립트 코드를 압축시켜주는 사이트입니다. 자바스크립트 코드를 복사해서 붙여넣고 Compress 버튼을 누르면 주석과 공백을 제거해줍니다.

![JavaScript Compress](/assets/post-images/js-compress/compress.png)

## 주의사항

### 세미콜론 추가
자바스크립트 코드는 세미콜론을 생략해도 제대로 동작합니다. 그러나 압축을 하면서 공백을 제거하게 되면 코드의 의미가 모호해져서 문법 에러가 발생합니다.

![No Semicolon](/assets/post-images/js-compress/semicol.png)

VS Code와 같은 에디터를 사용할 경우, 세미콜론이 빠진 부분(문법에러)을 알려줍니다. (**ctrl + j**)

![Problems](/assets/post-images/js-compress/problem.png)

### HTML 공백 체크
자바스크립트의 문자열로 HTML 코드를 작성하는 경우에 태그와 태그 사이에 공백이 사라지면 화면 디자인이 바뀔 수 있습니다.

* 잘못된 화면
![Destroy2](/assets/post-images/js-compress/destroy2.png)

필요한 위치에 공백을 추가하거나, `\n` 과 같은 공백문자를 추가하면 해결됩니다.

![Space](/assets/post-images/js-compress/space.png)

* 복구된 화면
![Origin2](/assets/post-images/js-compress/origin2.png)

### HTML 공백 체크 2
태그의 속성(Attribute)를 설정할 때, 따옴표를 사용하지 않고 공백으로만 속성을 설정했을 때 문제가 생깁니다. 따옴표를 추가해줍시다.(ex. **width=300** => **width="300"**)

* 잘못된 화면
![Destroy](/assets/post-images/js-compress/destroy.png)

* 복구된 화면
![Origin](/assets/post-images/js-compress/origin.png)

{% include ad-contents.html %}

## 결과
![befor after](/assets/post-images/js-compress/before-after.png)

41.5KB 에서 25.9KB 로, 약 62%로 코드 크기가 줄어들었습니다.

## Reference
* [JavaScript Compressor](http://javascriptcompressor.com/){:target="_blank"}