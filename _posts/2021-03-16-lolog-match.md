---
title: "[LoLog.me] 리그오브레전드 전적 상세 조회 기능 추가"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-03-16T14:04:30+09:00
categories:
  - LoL
tags:
  - League of Legend
  - LoLog.me
  - Node.js
permalink: /lol/lolog-match/
---
---
[LoLog.me](https://lolog.me/){:target="_blank"} 에서 이제 개별 전적을 조회할 수 있습니다. 유저들에게 가장 친숙한 리그오브레전드 게임 클라이언트의 매치 히스토리 UI를 본따서 제작하였습니다. 그리고 유저들이 궁금해할 만한 정보들을 추가했습니다. 또한 닉네임을 클릭하면 해당 유저를 바로 검색할 수 있습니다.
<!--more-->

{% include ad-contents.html %}

## 패치내역

* 리그오브레전드 클라이언트
<img src="/assets/post-images/match/client.png" alt="client" style="width:100%; border: 1px solid #24292e; margin-bottom: 10px;"/>

* [LoLog.me](https://lolog.me/){:target="_blank"}
<img src="/assets/post-images/match/win.png" alt="Win LoLog" style="width:100%; border: 1px solid #24292e; margin-bottom: 10px;"/>
<img src="/assets/post-images/match/lose.png" alt="Lose LoLog" style="width:100%; border: 1px solid #24292e; margin-bottom: 10px;"/>

매치 히스토리 데이터는 여러가지의 정보가 결합된 복잡한 데이터입니다. 이로 인해 유저들이 정보를 얻는데 피로하지 않도록 리그오브레전드 클라이언트의 매치 히스토리의 UI를 본땄습니다. 게임이 끝날 때마다 보는 친숙한 화면을 구성함으로써, 유저들의 혼란을 줄이고 편하게 정보를 얻을 수 있도록 했습니다.

추가로 화면을 너무 복잡하지 않게 하는 선에서 유저들이 자주 찾는 정보를 추가했습니다. 메인룬 옆에 보조룬 아이콘을 추가시켰고, 인게임 정보창에서 볼 수 있는 것처럼 와드 아이콘 위에 시야점수를 표시했습니다. 추가로 와드 구매/설치/제거 횟수, 킬 관여율, 분당 CS 는 툴팁으로 추가하였습니다. 또한 유저 닉네임을 클릭하면 해당 유저의 페이지로 바로 이동합니다.

매치 히스토리 UI가 기존의 [LoLog.me](https://lolog.me/){:target="_blank"} 사이트의 UI와 자연스럽게 어우러질 수 있도록, 승패에 따라 테마색을 정하고 현재 열려있는 매치 히스토리의 로그 아이콘의 색상을 변경시켰으며, 동일한 색상으로 매치 히스토리 창의 테두리를 추가했습니다.

{% include ad-contents.html %}

## 향후 계획

### 챔피언별 데미지 그래프 추가
챔피언별 데미지 그래프는 많은 유저들이 가장 궁금해하는 정보이고, 해당 게임의 핵심을 한눈에 확인할 수 있는 매우 중요한 정보입니다. 챔피언별 데미지를 현재 매치 히스토리 정보창에 추가했을 때, 화면이 너무 복잡해졌습니다. 그래서 오른쪽 위에 토글버튼을 만들고 데미지 그래프를 위한 새로운 창을 띄워주는 방식으로 데미지 정보를 제공할 계획입니다. (리그오브레전드 클라이언트에서도 비슷한 방식으로 데미지 그래프를 확인할 수 있으니, 유저에게 더욱 친화적인 방식이라고 생각됩니다)

### 툴팁 추가
룬, 챔피언, 스펠, 아이템은 아이콘으로만 표시되어 있습니다. 이에 대한 자세한 정보가 궁금한 유저가 있을 것입니다. 따라서 아이템 등에 대한 자세한 정보를 제공하는 툴팁을 만들어야 합니다.

### 모바일 반응성
이번에 추가된 전적상세 정보는 PC화면에 최적화 되어있습니다. 모바일 사용자들을 위해서는 좀더 압축된 정보창을 제공해야합니다.

### 최근전적 통계
전적 상세정보를 제공하기 위해, Riot API를 효율적으로 사용하여 Match 데이터를 가져오는 기능을 구현했습니다. 이에 따라 좀더 자세한 정보를 가진 매치 로그를 DB에 저장할 수 있게 되었고, 최근경기에 한해서 더 자세한 통계를 제공할 수 있게 되었습니다. 최근 경기의 승패 정보와 KDA 정보를 한눈에 확인할 수 있게 하고, 이에 대한 통계 그래프를 제공할 예정입니다.

{% include ad-contents.html %}

## [LoLog.me](https://lolog.me/){:target="_blank"}

## 이전 포스트
* [[LoLog.me] 한달간 업데이트 내역](/lol/lolog-monthly/){:target="_blank"}