---
title: "[LoLog.me] 한달간 업데이트 내역"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-03-01T12:07:30+09:00
categories:
  - LoL
tags:
  - League of Legend
  - LoLog.me
  - Node.js
permalink: /lol/lolog-monthly/
---
---
[LoLog.me](https://lolog.me/){:target="_blank"} 사이트를 제대로 만들어 보겠다는 [포스트](/lol/lolog-me){:target="_blank"}를 업로드한 후로 딱 한달이 지났습니다. 그동안 업데이트한 내용들을 정리해볼까 합니다.
<!--more-->

{% include ad-contents.html %}

## Preview
[LoLog.me](https://lolog.me/){:target="_blank"}

### 메인 페이지

* Before
<img src="/assets/post-images/lolog/homepage.png" alt="Homepage" style="width:100%; border: 1px solid #24292e; margin-bottom: 10px;"/>

* After
<img src="/assets/post-images/lolog-monthly/mainnew.png" alt="Hompage New" style="width:100%; border: 1px solid #24292e; margin-bottom: 10px;"/>

### User 페이지

* Before
<img src="/assets/post-images/lolog/userpage.png" alt="Userpage" style="width:100%; border: 1px solid #24292e;"/>

* After
<img src="/assets/post-images/lolog-monthly/usernew.png" alt="Userpage New" style="width:100%; border: 1px solid #24292e;"/>

{% include ad-contents.html %}

### 모바일 페이지

* Before
<center>
<img src="/assets/post-images/lolog/mobilehome.png" alt="mobilehome" style="width:30%; border: 1px solid #24292e; margin-bottom: 20px;"/>
<img src="/assets/post-images/lolog/mobileuser.png" alt="mobileuser" style="width:30%; border: 1px solid #24292e;margin-bottom: 20px;"/>
<img src="/assets/post-images/lolog/mobileuser2.png" alt="mobileuser2" style="width:30%; border: 1px solid #24292e;margin-bottom: 20px;"/>
</center>

* After
<center>
<img src="/assets/post-images/lolog-monthly/mainmobilenew.png" alt="mobilehome new" style="width:30%; border: 1px solid #24292e; margin-bottom: 20px;"/>
<img src="/assets/post-images/lolog-monthly/usermobilenew.png" alt="mobileuser new" style="width:30%; border: 1px solid #24292e;margin-bottom: 20px;"/>
<img src="/assets/post-images/lolog-monthly/usermobilenew2.png" alt="mobileuser2 new" style="width:30%; border: 1px solid #24292e;margin-bottom: 20px;"/>
</center>

{% include ad-contents.html %}

## Riot API Production Key 승인
먼저 계획했던 대로 Riot API를 이용해 사이트가 동작하도록 웹앱의 코드를 전체적으로 리팩토링했습니다. Riot API가 생각보다 응답속도가 굉장히 빠릅니다. (체감상 디스코드봇을 만들 때 사용했던 YouTube API보다 빠른 것 같았습니다) 그리고 Node.js의 특성상 병렬적으로 API를 호출하고 데이터를 처리하기 수월했습니다. 그 결과, 크롤링을 사용해서 데이터를 가져왔을 때보다 사이트의 응답속도가 몇배는 더 빨라졌습니다.

리팩토링을 마친 후에 Riot API Production Key를 신청하고 승인받았습니다. 이제 정식으로 Riot API를 사용하여 웹서비스를 제공할 수 있게 된 것입니다.

Riot API 관련 포스트
* [Riot API 사용법 - 계정 생성 & 소환사 정보 가져오기](/lol/riot-api/){:target="_blank"}

* [Riot API - Production API Key 발급받기(Register Product)](/lol/production-key/){:target="_blank"}

* [Riot API - Production API Key 승인!](/lol/production-key-approved/){:target="_blank"}

* [Riot API Match-V4, matchHistoryUri가 뭘까?](/lol/riot-matchhistoryuri//){:target="_blank"}

## 상세 검색 옵션 추가
1. 지역 설정을 변경할 수 있고, 리그오브레전드가 서비스하는 모든 지역의 소환사 정보를 조회할 수 있습니다.

2. 시즌별, 기간별로 전적 기록을 조회할 수 있습니다.

3. 포지션별, 챔피언별로 전적 기록을 조회할 수 있습니다.

4. 기존에 지원하던 솔랭, 자랭 등의 게임 모드별 검색 기능에서, 이제 모든 게임모드를 조회 가능합니다.

{% include ad-contents.html %}

## DB 연동
Riot API를 이용해서 전적 로그를 가져오기 위해서는 100 게임당 1개의 API 호출이 필요합니다. 유저 한명당 1000게임 이상의 전적 로그를 한번에 가져와야할 경우가 많아서 매번 API를 호출할 경우 Riot API의 Rate Limit에 금방 도달해버릴 것입니다. 또한 사이트 로딩 속도도 느려지겠죠.

그래서 API를 통해 가져온 유저 데이터 및 전적 로그 데이터를 DB에 저장해두었습니다. 동일한 유저를 검색했을 때에는 API를 호출하지 않고 DB에서 빠르게 데이터를 가져옵니다. DB서버는 AWS RDS MySQL 서버를 이용중입니다. (AWS 프리티어 기간이라 EC2와 함께 무료로 이용중입니다)

현재까지는 DB 응답이 빠르게 잘 이루어지고 있습니다. 현재 유저 41명 기준 약 6MB 정도의 용량을 차지하고 있고, 유저가 늘어날 경우에도 잘 작동할 지는 지켜봐야할 것 같습니다.

MySQL 관련 포스트
* [MySQL INSERT 한번에 하기 (성능 대폭 향상)](/mysql/insert-multi/){:target="_blank"}

## 포지션별, 챔피언별 도넛차트 추가
유저별로 수많은 게임 로그 데이터를 어떻게 시각화하면 흥미로울 지를 고민하였습니다. 롤에서 가장 중요한 포지션(라인)과 챔피언에 관한 정보를 차트로 보여주고 싶었습니다. 이제 선택한 기간 동안에 플레이한 포지션과 챔피언의 비율을 도넛차트로 한눈에 확인할 수 있습니다. 특정 포지션을 선택할 경우, 해당 포지션으로 플레이한 챔피언의 비율도 확인해볼 수 있습니다.

차트는 [Google Charts](https://developers.google.com/chart){:target="_blank"} 라이브러리를 이용해서 구현했습니다. 공개된 라이브러리를 잘 활용하는 것도 개발자의 중요한 스킬인 것 같습니다. 라이브러리를 잘 이해하고 활용하면 개발 효율이 훨씬 상승합니다!

차트 관련 포스트
* [JavaScript 차트 만들기[Frappe Charts]](/js/frappe-charts/){:target="_blank"} ([롤로그미](https://lolog.me){:target="_blank"}에 Frappe Charts를 활용하지는 않았습니다.)
* [JavaScript 도넛 차트 만들기[Google Charts]](/js/charts-donut/){:target="_blank"}

{% include ad-contents.html %}

## UI 대폭 변경
* 메인페이지 배경색 변경

* 로고 글씨체 변경 (상업적으로 이용 가능한 무료폰트: [Bombardier](https://www.1001freefonts.com/bombardier.font){:target="_blank"})

* Favicon 추가 (브라우저 탭에 뜨는 작은 아이콘, 참고 사이트: [Favicon Generator](https://realfavicongenerator.net/){:target="blank"})

* Footer 추가 (사이트 맨 아래 문구)

* 그외 여러 디자인 (검색창, 버튼, 로그 숨기기 등)

여러 브라우저에 대응하려다보니 까다로운 부분이 많았습니다. 일단 데스크탑 기준 크롬, 사파리, 엣지, 파이어폭스를 지원하고 모바일 기준 크롬, 사파리를 지원합니다. 이렇게 되면 90% 이상의 유저를 지원하게 됩니다!

## 다국어 지원
아직 다국어라하기에는 좀 무리가 있지만, 현재 한국어와 영어를 지원합니다. npm의 [i18n](https://www.npmjs.com/package/i18n){:target="_blank"} 모듈을 이용했습니다.

## 향후 계획
이상하게 업데이트를 하면 할수록 할일이 더 많아집니다... 아직 수정해야할 부분도 많은데 추가하고 싶은 기능과 서비스가 점점 많아지고 있습니다.

{% include ad-contents.html %}

### 패치가 필요한 부분
* 모바일에서 도넛차트가 너무 크게 보입니다.

* 현재 소환사를 검색하면 1년치 데이터가 기본값으 조회됩니다. 모바일 기기는 데스크탑만큼 성능이 좋지 않습니다. 모바일 기기에서 검색할 시에는 조회 기간을 더 짧게 설정하여 페이지 로딩 속도를 향상시켜야할 필요가 있습니다.

* 다국어 지원은 하지만 접속 지역에 따라서 리그오브레전드 서버 지역이 자동으로 바뀌지 않습니다. 현재는 한국서버가 검색 기본값으로 항상 선택되어 있습니다. 지역에 따라서 언어가 바뀌듯이 검색 지역 또한 자동으로 설정되도록 해야합니다.

### 추가하고 싶은 기능
* 전적 상세 정보 - 전적에 대한 상세 정보를 보여주고 싶습니다. 그러나 전적 상세 정보는 1 게임당 1번의 API 호출이 필요합니다. 그래서 상위 몇개의 전적에 한해서만 상세 정보를 제공해야할 것입니다. 또한 전적 상세 정보를 DB에 저장하게 될 경우 용량을 훨씬 더 차지할 것입니다. 이를 어떻게 해결할 지도 생각해 보아야 합니다.

* 정글 동선 분석 - 여러 유저들의 게임 플레이 기록을 분석하여 정글 챔피언별로 효과적인 정글 동선을 분석해보고 싶습니다. 정글 동선은 정글러에게 있어서 가장 중요한 부분입니다. 이를 데이터에 기반하여 분석하면 유용한 데이터를 만들어낼 수 있을 것 같습니다. (제가 정글러라서 저에게도 큰 도움이 될 겁니다!)

* 구글 애드센스 - 구글 애드센스를 신청했으나 반려되었습니다. 아직은 부족한 부분이 많은 것 같습니다. 상세 전적 기능까지 구현한 뒤에 다시 신청해볼 계획입니다.

{% include ad-contents.html %}

## Reference
* [LoLog.me](https://lolog.me/){:target="_blank"}

* [LoLog.me - 롤 전적검색 사이트](/lol/lolog-me){:target="_blank"}

* [Riot API 사용법 - 계정 생성 & 소환사 정보 가져오기](/lol/riot-api/){:target="_blank"}

* [Riot API - Production API Key 발급받기(Register Product)](/lol/production-key/){:target="_blank"}

* [Riot API - Production API Key 승인!](/lol/production-key-approved/){:target="_blank"}

* [Riot API Match-V4, matchHistoryUri가 뭘까?](/lol/riot-matchhistoryuri//){:target="_blank"}

* [MySQL INSERT 한번에 하기 (성능 대폭 향상)](/mysql/insert-multi/){:target="_blank"}

* [JavaScript 차트 만들기[Frappe Charts]](/js/frappe-charts/){:target="_blank"}

* [JavaScript 도넛 차트 만들기[Google Charts]](/js/charts-donut/){:target="_blank"}

* [Google Charts](https://developers.google.com/chart){:target="_blank"}

* [Bombardier](https://www.1001freefonts.com/bombardier.font){:target="_blank"}

* [Favicon Generator](https://realfavicongenerator.net/){:target="blank"}

* [i18n](https://www.npmjs.com/package/i18n){:target="_blank"}

{% include ad-contents.html %}