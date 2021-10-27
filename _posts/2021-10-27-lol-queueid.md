---
title: "[Match-V5] 게임 타입(큐 타입) 알아내기 (Riot API)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-27T20:52:30+09:00
categories:
  - LoL
tags:
  - Riot API
  - League of Legend
permalink: /lol/queue-id/
---
---

Match-V5 에서 큐 타입 (솔랭, 자랭, 일반 등) 은 info 오브젝트의 queueId 필드에 나와있습니다.
<!--more-->

## queueId
queueId 필드는 int를 반환하는데, 이는 각각 큐 타입의 고유 넘버입니다. 큐 타입에 대한 자세한 정보는 공식 문서에서 확인할 수 있습니다.
* [queues.json](https://static.developer.riotgames.com/docs/lol/queues.json){:target="_blank"} 

## 예시
[LoLog.me](https://lolog.me/){:target="_blank"} 에서는 queueId를 다음과 같이 파싱합니다.
```js
const QUEUETYPE = {
    400: 'norm', //Normal Draft Pick
    420: 'solo',
    430: 'norm',
    440: 'flex',
    450: 'aram',
    700: 'clash',
    800: 'ai',  // Deprecated
    810: 'ai',  // Deprecated
    820: 'ai',  // Deprecated
    830: 'ai',
    840: 'ai',
    850: 'ai',
    900: 'urf',
    920: 'poro',
    1020: 'ofa',
    1300: 'nbg',
    1400: 'usb', // Ultimate Spellbook
    2000: 'tut',
    2010: 'tut',
    2020: 'tut',
}
```

한국어로 번역했을 때,
```js
const ko = {
  "solo": "솔랭",
	"norm": "일반",
	"aram": "칼바람",
	"flex": "자랭",
	"nbg": "돌넥",
	"usb": "궁주문서",
	"urf": "URF",
	"ofa": "단일",
	"ai": "AI대전",
	"poro": "포로왕",
	"tut": "튜토리얼",
	"etc": "기타",
	"clash": "격전"
}
```

## Reference
* [LoL Documents](https://developer.riotgames.com/docs/lol){:target="_blank"}
* [LoLog.me](https://lolog.me){:target="_blank"}
