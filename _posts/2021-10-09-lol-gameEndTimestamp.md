---
title: "[Match-V5 업데이트] gameDuration 변경 및 gameEndTimestamp 추가 (Riot API)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-09T00:04:30+09:00
categories:
  - LoL
tags:
  - Riot API
  - League of Legend
permalink: /lol/game-duration/
---
---
{% include ad-contents.html %}

Match-V4 에서 Match-V5 로 넘어가면서 gameDuration(게임 길이) 값이 초단위에서 밀리초단위로 변경되었습니다.

그러나 11.20 패치 이후로 다시 초단위로 돌아가게 되었고 gameEndTimestamp 라는 필드가 새로 생겨났습니다.
<!--more-->

## Riot API Match-V5 의 공식 안내
![new info](/assets/post-images/lol-game-duration/new-info.png)

Match-V4 에서 Match-V5 로 넘어가면서 gameDuration의 계산 방식이 바뀌었었나봅니다. 이번 11.20 패치 이후부터는 다시 이전의 방식으로 돌아가게 되었습니다.

## gameDuration 계산 방식

### 1. Match-V4, 11.20 이후의 Match-V5

게임에 참여한 플레이어들 중에서 가장 오랜 시간동안 게임을 플레이한 유저의 게임 플레이 시간이 gameDuration 이 됩니다. (초 단위의 값입니다)

### 2. 11.20 이전의 Match-V5
**gameEndTimestamp - gameStartTimestamp**: 
gameEndTimestamp 는 라이엇 서버에서 게임이 종료되었다고 판단되는 시간입니다. gameStartTimestamp 는 게임이 시작된 시간 입니다. (밀리초 단위의 값입니다)

{% include ad-contents.html %}
## 이전 방식으로 돌아간 이유
1번 방식은 결국 마지막 유저가 게임에서 나간 시간이 gameDuration이 됩니다. 2번 방식의 경우에도 사실상 마지막 유저가 게임에서 나간 시간이 게임이 종료된 시간이니 같은 값의 gameDuration을 갖게 될 것입니다.

그러나 라이엇 서드파티 개발자 커뮤니티에 의하면 특정 서버나 플랫폼의 오류로 인해서 gameEndTimestamp가 실제 게임이 종료된 시간보다 길게 기록되는 경우가 종종 있다고 합니다.

이러한 이유로 1번 방식의 gameDuration은 종종 부정확한 값을 가지게 되었고 다시 원래의 방식으로 돌아가게 된 것입니다.

## 대응
11.20 패치로 인해 이전 방식으로 돌아갔지만 이전의 게임 로그들은 바뀌지 않았습니다. 따라서 gameDuration 을 불러올 때에 초단위로 쓰였는지 밀리초 단위로 쓰였는지 구분해줄 필요가 있습니다.

라이엇에서 추천하는 방식은 gameEndTimestamp 필드의 유무로 판단하는 것입니다. 11.20 패치 이후로 gameEndTimestamp 라는 필드가 생겼으니 해당 필드가 존재할 경우 gameDuration 은 초단위로 기록되었을 것이고 존재하지 않을 경우 밀리초 단위로 기록되었을 것입니다.

아래는 예제 코드 입니다. (gameDuration 파싱 이후 초단위로 처리합니다)
```js
/** gameDuration & gameEndTimestamp Before or After 11.20 */
if(!data.info.gameEndTimestamp) data.info.gameDuration /= 1000;
```

## Reference
* [MATCH-V5](https://developer.riotgames.com/apis#match-v5/GET_getMatch){:target="_blank"}

{% include ad-contents.html %}