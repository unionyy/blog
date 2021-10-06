---
title: "MySQL INSERT 한번에 하기 (성능 대폭 향상)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-02-20T01:07:30+09:00
categories:
  - MySQL
tags:
  - MySQL
  - Database
  - LoLog.me
permalink: /mysql/insert-multi/
---
---

{% include ad-contents.html %}


MySQL에서 여러개의 INSERT 쿼리를 통합시켜 하나의 쿼리문으로 만들면 성능이 대폭 향상됩니다. (하나의 table에 대한 쿼리문일 경우)
<!--more-->


## MySQL INSERT 쿼리 문법
```sql
INSERT INTO tbl_name (a,b,c)
    VALUES(1,2,3), (4,5,6), (7,8,9);
```

## 성능 테스트 ([LoLog.me](https://lolog.me){:target="_blank"})

Riot API는 플레이어의 게임 데이터를 100경기씩 반환해줍니다. LoLog.me 의 웹서버는 1년치 게임 데이터를 가져와 하나씩 DB에 저장합니다. 게임 하나마다 INSERT 쿼리를 전송할 경우, 유저의 게임 플레이 횟수가 많아질수록 로드가 매우 커지게 됩니다.

![one by one](/assets/post-images/insert-multi/one-by-one.png)

위는 페이커 선수의 최근 1년 게임데이터를 가져오면서 찍은 로그입니다. Riot API로 게임데이터를 가져온 후로 부터 DB에 게임데이터를 모두 INSERT하고 다시 데이터를 가져오기까지 걸리는 시간을 측정했습니다.

총 1800경기를 DB에 INSERT하고, SELECT문을 통해 다시 가져오기까지 걸리는 시간은 대략 12초 정도 였습니다. 웹사이트에서 검색 결과를 기다리기에는 너무 큰 시간입니다.

INSERT 쿼리를 게임 단위에서 Riot API 단위(최대 100경기) 로 변경한 결과,

![multi](/assets/post-images/insert-multi/multi.png)

동일한 일을 처리하는데 걸리는 시간이 74ms로 측정되었습니다... 엄청난 수치의 성능향상입니다. 전송되는 쿼리의 갯수는 1/100로 줄었지만 쿼리 하나 하나의 처리량이 많아졌기 때문에 100배의 성능향상을 기대하지도 않았습니다. 그러나 무슨이유에서인지 100배 이상의 성능향상이 있었습니다.

{% include ad-contents.html %}

## 가설

곰곰이 생각해본 결과 제가 생각해낸 원인은 INDEX 입니다. 저는 게임데이터에서 user_id와 timestamp(게임시작시간)을 결합하여 INDEX를 만들고, 이를 PRIMARY KEY 로 설정했습니다. 그래서 게임데이터가 하나씩 INSERT 될 때마다 INDEX가 업데이트 되고, 같은 user_id를 가진 데이터가 늘어남에 따라 INSERT 쿼리를 처리하는 데에 걸리는 시간이 점점 늘어나게 되는 것입니다.

이는 그저 저의 추측일 뿐이며, 구체적인 알고리즘은 저도 알지 못합니다. 정확한 원인을 알고 계신 분이 있다면 알려주세요 ㅎㅎ

조만간 DB를 제대로 공부해볼 계획입니다. DB와 MySQL을 공부하다보면 정확한 원인을 찾을 수 있겠지요. 오늘은 일단 여기까지하고 쉬어야겠습니다... 뭔가 찾으면 다시 포스팅 하겠습니다.

## Reference
* [13.2.6 INSERT Statement](https://dev.mysql.com/doc/refman/8.0/en/insert.html){:target="_blank"}

{% include ad-contents.html %}

---
## 추가1

참고할 만한 자료
* [BULK INSERTING - MYSQL 다량의 데이터 넣기](https://dev.dwer.kr/2020/04/mysql-bulk-inserting.html){:target="_blank"}

## 추가2

INSERT 쿼리가 처리되기 전에 SELECT 쿼리를 전송한 것이 성능을 저하시킬 수도 있다는 생각이 들었습니다. 이부분도 고려하여 좀 더 공부해보아야겠습니다.