---
title: "[LoLog.me] 리그오브레전드 최근 전적, 통계 업데이트"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-04-03T21:33:30+09:00
categories:
  - LoL
tags:
  - League of Legend
  - LoLog.me
  - Node.js
permalink: /lol/lolog-recent/
---
---
{% include ad-contents.html %}


[LoLog.me](https://lolog.me/){:target="_blank"} 에서 이제 최근 전적을 한눈에 볼 수 있고, 승률 통계를 확인할 수 있습니다.
<!--more-->

## 패치내역

기존의 사이트는 검색한 유저의 모든 전적을 아이콘으로 표시했었습니다. 그러나 로그 데이터를 나열하기만한 정보는 별로 유용하지 않았습니다.

* 이제 한번 검색할 때마다 20개의 기록만을 보여주고, 한눈에 볼 수 있는 더 자세한 정보를 제공합니다. (+ 버튼을 누르면 전적이 20개씩 추가로 로드됩니다)

* 조회한 게임들의 승률 정보를 추가하여, 유저가 자신의 게임 플레이 스타일을 눈으로 확인할 수 있도록 했습니다.

![recent match](/assets/post-images/lolog-recent/overview.PNG)

전적 정보는 다른 전적 검색 사이트들과 비슷하게 구성하였고 최소한의 정보만을 제공하여 유저들이 보기 편하게 만들었습니다.

왼쪽에는 승률그래프를 제공합니다. 다른 사이트들에서는 제공하지 않는 블루팀, 레드팀별 승률 및 시간대별 승률 그래프를 제공합니다.

[LoLog.me](https://lolog.me/){:target="_blank"}에서는 날짜별, 챔피언별, 라인별 전적 데이터를 조회할 수 있습니다. 여기에 그에 따른 승률 정보까지 제공하게 되어서 유저들이 좀 더 흥미로워할만한 정보를 제공하게 되었습니다.

{% include ad-contents.html %}

## 향후 계획

* 챔피언별 데미지 그래프 추가

* 툴팁 추가

* 모바일 반응성

## 이전 포스트
* [[LoLog.me] 리그오브레전드 전적 상세 조회 기능 추가](/lol/lolog-match/){:target="_blank"}