---
title: "[LoLog.me] Match-V5 업데이트"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-05T11:13:30+09:00
categories:
  - LoL
tags:
  - League of Legend
  - LoLog.me
  - Node.js
permalink: /lol/lolog-matchv5/
---
---
Riot API 의 Match-V4 지원이 종료되었습니다. 그래서 [LoLog.me](https://lolog.me/){:target="_blank"} 서버를 Match-V5 에 맞게 업데이트했습니다.

<!--more-->

{% include ad-contents.html %}

## 난잡한 코드

업데이트를 하기 위해 코드를 살펴보니 코드 구조가 굉장히 난잡하고 유지보수를 하기 어렵게 짜여져 있는 것을 발견했습니다.

아무래도 Node.js 와 기본적인 웹프로그래밍을 공부하면서 실습용으로 만든 사이트였기 때문에 대강 설계하고 손가는 대로 코드를 짰던 것 같습니다.

이번에 Match-V5 업데이트를 하면서 난잡했던 코드 구조를 깔끔하게 개선하여 리팩토링했습니다. 앞으로는 Riot API 가 바뀌어도 업데이트가 수월할 것입니다.

아직 개선해야할 부분이 정말 많아서 천천히 구조화 시켜나갈 생각입니다.

## 전적 그래프 삭제

안타깝게도 Match-V4 에서 Match-V5 로 넘어가면서 전적 그래프를 제공할 수 없게 되었습니다.

* 삭제된 전적 그래프
![graph](/assets/post-images/lolog-matchv5/graph.png)

{% include ad-contents.html %}

전적 그래프로 게임을 얼마나 했는지 한눈에 확인하는 전적 그래프 기능은 [LoLog.me](https://lolog.me/){:target="_blank"} 의 근본 기능이었습니다.

전적 그래프로부터 모든 기능이 파생됐고, 웹사이트의 이름이 LoLog.me (롤 로그 미) 인 이유 또한 전적 그래프 때문이었습니다.

Match-V4 에서는 유저의 간단한 게임 로그를 최대 100개씩 가져올 수 있었습니다. 그러나 Match-V5 로 업데이트 되면서, 유저당 최대 100개씩 가져올 수 있는 정보는 Match ID 밖에 없었습니다.

이말은 전적 그래프 기능을 계속해서 제공하기 위해서는 Riot API 를 예전보다 100배 더 호출해야 한다는 뜻이고, 이는 Rate Limit 과 반응속도 등의 이유로 불가능한 일입니다.

그래서 아쉽지만 전적 그래프 제공을 중단하기로 했고 [LoLog.me](https://lolog.me/){:target="_blank"} 에서 전적 그래프를 제거하였습니다.

{% include ad-contents.html %}

## 바뀐 웹사이트

![graph](/assets/post-images/lolog-matchv5/new.png)

전적 그래프를 삭제하니 오히려 페이지가 깔끔해졌다는 장점도 있네요.

다만 전적 그래프에 통합돼있던 큐 타입별 전적 검색 기능이 사라져 버렸습니다. 이 기능은 다시 깔끔하게 구현하여 추가해야겠습니다.

+) 웹페이지 테마가 게임 화면 대비 너무 밝아서 게임 화면과 번갈아서 볼 경우 눈이 부실 때가 있습니다. 웹 디자인도 추후에 개선해야겠습니다.

## 앞으로 해야할 일

* 코드 구조 개선 작업 지속
* 큐 타입별 검색 기능 구현
* 웹 디자인 개선 (어둡게)

{% include ad-contents.html %}