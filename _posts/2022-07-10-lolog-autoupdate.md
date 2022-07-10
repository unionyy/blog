---
title: "[LoLog.me] Riot Data 자동 업데이트 기능 추가"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-10T17:10:30+09:00
categories:
  - LoL
tags:
  - League of Legend
  - LoLog.me
  - Node.js
permalink: /lol/lolog-autoupdate/
---
---

리그오브레전드 패치가 있을 때 마다(챔피언 추가, 아이템 변경 등) [LoLog.me](https://lolog.me/){:target="_blank"}도 업데이트가 필요했습니다.

코드를 개선하여 1시간 마다 데이터가 자동으로 업데이트 되도록 했습니다.

<!--more-->

## 변경 사항
1. 아이콘(프로필, 챔피언, 룬): 항상 최신 버전 아이콘을 가져옵니다. (현재 12.12.1, 자동 업데이트, DDragon CDN 이용)
2. 아이콘(아이템): 플레이 시점 버전의 아이템 아이콘 -> 최신 버전 아이템 아이콘 (아이템 아이콘은 BananaCDN latest 이용)
3. 챔피언 정보: 신규 챔피언 출시 직후 바로 적용 (자동 업데이트)
4. 룬 정보: 룬 정보 변경 시 바로 적용 (자동 업데이트)

## 앞으로 업데이트 방향
최근 몇 개월 간 사이트가 방치되고 있었습니다. 방치되는 기간 동안 신규 챔피언이 추가되고 신규 룬도 추가되었는데, [LoLog.me](https://lolog.me/){:target="_blank"}에는 적용되지 않았습니다.
그래서 가장 먼저 한 작업이 게임이 업데이트될 때마다 [LoLog.me](https://lolog.me/){:target="_blank"}가 자동으로 업데이트되도록 하는 것이었습니다.

다행이 큰 문제 없이 자동 업데이트 기능을 추가할 수 있었습니다. 오랜만에 코드를 보니 개선해야할 부분들이 많이 보여, 코드 개선 작업도 꾸준히 진행해야할 것 같습니다.

그리고 지난 업데이트([Match-V5 업데이트](/lol/lolog-matchv5/){:target="_blank"})에서 제거한 기능들을 다시 추가해보려고 합니다.
