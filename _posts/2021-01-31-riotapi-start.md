---
title: "Riot API 사용법 - 계정 생성 & 소환사 정보 가져오기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-01-31T16:34:30+09:00
categories:
  - LoL
tags:
  - Riot API
  - League of Legend
permalink: /lol/riot-api/
---
---
리그오브레전드를 해봤다면 OP.GG, YOUR.GG 등 전적검색 사이트를 이용해본 경험이 있을 겁니다. 전적검색 사이트들은 Riot에서 제공하는 Riot API를 이용하여 유저들의 정보를 가져옵니다.

Riot API를 이용해서 저만의 롤 전적검색 사이트를 만들어 보려합니다.

이 포스팅에서는 Riot API의 기본적인 사용법을 살펴보고 간단히 테스트해보도록 하겠습니다.

<!--more-->
{% include ad-contents.html %}

## 계정생성
[라이엇 개발자 페이지](https://developer.riotgames.com/){:target="_blank"}에 접속하여 본인의 롤 계정으로 로그인하거나 새 계저을 만들어줍니다.

![roitdeveloper](/assets/post-images/riotapi-start/homepage.png)

로그인을 하면 약관 동의 화면이 나옵니다. 동의하면 라이엇 개발자 계정이 생성됩니다.

![policy](/assets/post-images/riotapi-start/policy.png)

RIOT API KEY가 생성되었습니다. 지금 생성된 KEY는 DEVELOPMENT API KEY(개발용) 이므로 RATE LIMITS이 작게 설정되어있고, 공개 서비스에 사용할 수 없습니다. 공개서비스에 사용을 원한다면 API 신청을 하여 승인을 받아야합니다.

![created](/assets/post-images/riotapi-start/created.png)

## 테스트

라이엇 개발자 페이지에서는 Riot API의 사용법을 알아보고 원하는 API를 간편하게 테스트해볼 수 있습니다.

{% include ad-contents.html %}

### 소환사명으로 검색하기

페이지 상단의 `API` 탭으로 들어가서 `SUMMONER-V4` 항목을 선택하고, 두번째 항목인 `by-name`을 클릭해줍니다.

![testing](/assets/post-images/riotapi-start/testing.png)

그러면 하단에 해당 API를 테스트해볼 수 있는 칸이 나옵니다.
`PATH PARAMETERS` 탭의 `summonerName` 에 검색할 소환사의 이름을 입력하고 `EXECUTE REQUEST` 를 클릭합니다.

저는 Faker 선수의 소환사명인 Hide on bush를 검색해보겠습니다.

![summonertest](/assets/post-images/riotapi-start/summoner.png)

RESULT BODY 탭에서 API의 응답을 확인할 수 있습니다.

검색한 소환사의 id, accountId, puuid 를 얻었습니다. id, accountId의 경우 하나의 리전(KR)안에서만 고유한 값이고, puuid는 모든 리전에서의 고유한 값입니다.

그 외에도 소환사명, 레벨 등의 정보도 조회되었네요.
![summonerresult](/assets/post-images/riotapi-start/summonerresult.png)

이 중에서 본인이 사용할 API가 요구하는 id값을 사용하면 됩니다. 주의할 사항은 API의 사용자가 바뀌면(KEY가 바뀌면) id값들도 바뀌게 됩니다. 즉, id값들은 하나의 API KEY만을 위해 암호화된 값들입니다. (API KEY를 재발급 받는 경우에는 바뀌지 않습니다) - [참조](https://riot-api-libraries.readthedocs.io/en/latest/ids.html){:target="_blank"}


{% include ad-contents.html %}

### 소환사 리그 정보 조회하기 (티어, 승률 등)

`LEAGUE-V4` 탭에서 `by-summoner` 항목을 선택합니다.

![league](/assets/post-images/riotapi-start/league.png)

마찬가지로 `PATH PARAMETERS` 탭에서 파라미터 값을 입력하고 `EXECUTE REQUEST`를 클릭해줍니다.

저는 위에서 조회했던 Hide on bush 소환사의 id를 검색해보았습니다.

![leaguetest](/assets/post-images/riotapi-start/leaguetesting.png)

`RESPONSE BODY`를 보면 티어, 승리 횟수, 패배 횟수 등의 현시즌 리그 정보를 확인할 수 있습니다.

## Reference
* [DEVELOPER-RIOTGAMES](https://developer.riotgames.com/){:target="_blank"}
* [PUUIDs and Other IDs](https://riot-api-libraries.readthedocs.io/en/latest/ids.html){:target="_blank"}

{% include ad-contents.html %}