---
title: "Riot API Match-V4, matchHistoryUri가 뭘까?"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-02-27T21:14:30+09:00
categories:
  - LoL
tags:
  - Riot API
  - League of Legend
  - LoLog.me
permalink: /lol/riot-matchhistoryuri/
---
---
Riot API의 Match-V4의 리턴값 중에서 PlayerDto 안에 'matchHistoryUri'라는 값이 있습니다. 이 값을 어디에 사용할 수 있을까요?

<!--more-->

## 발견
[LoLog.me](https://lolog.me/){:target="_blank"} 사이트에 전적 정보를 제공하기 위해 Riot API의 Match-V4를 살펴보던 중 'matchHistoryUri'라는 값을 발견했습니다.
```json
{
  "participantId": 7,
  "player": {
      "platformId": "KR",
      "accountId": "hFlkJJh8PyYwQlyagva_cVk2-87VL4UGzwNjV9v0u-QB1lw",
      "summonerName": "유년이",
      "summonerId": "rs841I4dVrVYnXe1NsawNrm8nJJ9vUsI5QUdLkk99ZwzwaI",
      "currentPlatformId": "KR",
      "currentAccountId": "hFlkJJh8PyYwQlyagva_cVk2-87VL4UGzwNjV9v0u-QB1lw",
      "matchHistoryUri": "/v1/stats/player_history/KR/206932210",  // ???
      "profileIcon": 4594
  }
},
```
제가 필요한 기능이 Match History를 가져오는 것이었기 때문에 matchHistoryUri의 정체가 뭔지 알고 싶었습니다.

## 정체가 뭐냐
[라이엇 개발자 페이지](https://developer.riotgames.com/){:target="_blank"}를 뒤져봤지만 관련된 정보를 찾을 수 없었고 구글에 검색해도 제대로된 자료를 찾기 어려웠습니다.

일단 `https://acs.leagueoflegends.com`에 matchHistoryUri를 붙여넣으면 플레이어의 최근 10개의 전적을 볼 수 있다는 것을 알아냈습니다.

[https://acs.leagueoflegends.com/v1/stats/player_history/KR/206932210](https://acs.leagueoflegends.com/v1/stats/player_history/KR/206932210){:target="_blank"}

그러나 이 데이터가 라이엇에서 공식적으로 제공하는 데이터인지 알 수 없었고, 이것을 웹서비스에 이용해도 되는지도 알 수 없었습니다. 그리고 Riot 계정으로 로그인하지 않은 상태에서는 데이터를 요청할 수 없는 문제도 있었습니다.

## 디스코드 채널에서 답을 찾다

명확한 해답을 찾기 위해 라이엇 API 공식 디스코드 채널에 가입했습니다. 디스코드 채널에서 matchHistoryUri라는 키워드로 검색해보니 저와 비슷한 궁금증을 가진 사람들이 많았습니다. 그리고 이에 대한 해답을 얻을 수 있었습니다.

![Not Use ACS](/assets/post-images/riot-matchhistoryuri/notuseacs.png)

일단, ACS는 웹서비스에 이용하면 안됩니다. 이유는 세가지가 있습니다.
- Rate Limit을 알 수 없다. (안정적인 서비스 불가능)
- 암호화되지 않은 ID를 제공한다. (Riot API는 사용자별로 다르게 암호화된 ID를 사용합니다. 따라서 ACS와 API를 혼합해서 사용하기 어려울 것입니다.)
- 문제가 생기면 해결하기 어렵다.(?) (아마 ACS가 공식적으로 제공하는 기능이 아니기 때문에 issue 해결하는데 어렵다는 말 같습니다.)

## 결론
결론은 "사용하지 않는 것" 이 답입니다. 현재 라이엇에서 정식으로 제공하는 기능도 아니며, 활용하기도 어려울 것입니다.

전적 10개를 한번에 가져오는 기능은 현재 Match-V4 API에서 제공하지 않습니다. 여러개의 전적 데이터를 가져오기 위해서는 Match-V4 API를 여러번 동시에 호출하는 것이 현재로써는 최선의 방법으로 보여집니다.

[LoLog.me](https://lolog.me/){:target="_blank"}에서는 현재 개별 매치 데이터를 제공하고 있지 않습니다. (라이엇 공식 사이트로 연결해줍니다.) 하루빨리 개별 전적 데이터 제공 및 저장 기능을 구현해야겠습니다. (데이터가 워낙 많아 API와 DB를 적절히 활용하는 최적의 방법을 찾아야 합니다..!)

## Reference
* [DEVELOPER-RIOTGAMES](https://developer.riotgames.com/){:target="_blank"}

* [The New Match History](https://www.reddit.com/r/leagueoflegends/comments/27wax9/the_new_match_history/){:target="_blank"}