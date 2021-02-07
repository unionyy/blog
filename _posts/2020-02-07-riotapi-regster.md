---
title: "Riot API - Production API Key 발급받기(Register Product)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-02-07T20:40:30+09:00
categories:
  - LoL
tags:
  - Riot API
  - League of Legend
permalink: /lol/production-key/
---
---
Riot API로 공개서비스를 제공하기 위해서는 제품을 등록하고, Production API Key를 발급받아야 합니다. 
<!--more-->

이전 포스트, [LoLog.me - 롤 전적검색 사이트](/lol/lolog-me/){:target="_blank"}에서 계획한 대로 [LoLog.me](https://lolog.me/){:target="_blank"} 웹서버를 Riot API를 사용하는 방식으로 변경하였습니다. YOUR.GG를 크롤링하던 코드를 모두 제거하고 Riot API 모듈을 만들어 적용시켰습니다.

현재는 이것저것 생각했던 기능을 추가해보고 있는 중입니다. 아직 웹서비스를 정식으로 출시할 단계는 아니지만, 제품이 승인되고 Production Key가 승인되기 까지 2주정도 시간이 걸린다고 하여 미리 신청해 두었습니다. (반려된다면 다시 신청해야겠죠 ㅠ)


## 계정생성 및 테스트
[Riot API 사용법 - 계정 생성 & 소환사 정보 가져오기](/lol/riot-api/){:target="_blank"} 이전 포스팅에 자세히 설명해두었습니다.



## 제품 등록 - PRODUCT TYPE 선택하기
[라이엇 개발자 페이지](https://developer.riotgames.com/){:target="_blank"}에 로그인하고 대쉬보드에서 `REGISTER PRODUCT` 버튼을 클릭합니다.

PRODUCT TYPE 선택화면에서 원하는 타입을 선택합니다.
![product type](/assets/post-images/riotapi-register/producttype.png)

* PRODUCTION API KEY

공개 서비스를 제공할 때 필요한 API KEY입니다. Riot API를 이용한 어플리케이션이나 웹사이트를 제공하려할 때 선택합니다. 기본적으로 Rate Limit이 크게 설정되어 있고, 사용량이 많아지면 할당량 추가 신청을 할 수 있습니다. 제품 승인을 받고 API Key를 제공받기 위해서는 제품의 프로토타입과 웹사이트가 존재해야합니다.

* PERSONAL API KEY

개발자 혼자 사용하거나 소규모 비공개 그룹을 위한 제품에 사용할 수 있는 API Key입니다. Rate Limit이 PRODUCTION API KEY에 비해 작게 설정되어 있고, 추가 할당량을 획들할 수 없습니다.


## 제품 등록 - 신청서 작성 & 웹사이트 인증

원하는 PRODUCT TYPE을 선택하면 약관 동의 창이 뜹니다. 약관을 확인 후 동의하게 되면 신청서 작성 페이지가 열립니다. 저의 경우 웹사이트를 공개할 계획이기 때문에 PRODUCTION API KEY를 선택했습니다.

제품명, 제품 설명, 웹사이트 주소 등을 입력하고 제출하면 됩니다.

PRODUCTION API KEY를 선택한 경우, 접근 가능하고 본인이 관리할 수 있는 웹사이트를 제출해야 합니다. 제출 이후, 주어지는 코드를 `/웹사이트.주소/riot.txt` 경로에 올려 웹사이트를 인증하게 됩니다.

## 기다림...

신청서를 제출하고 웹사이트 승인을 끝마치면 이제 기다리는 일만 남았습니다. 유저 정보의 `GROUPS` 페이지에 들어가면, 신청한 앱의 간단한 정보를 확인할 수 있습니다. (자세한 정보는 `APPS` 페이지에 들어가면 확인할 수 있습니다.)

![pending](/assets/post-images/riotapi-register/pending.png)

저는 아직 승인이 되지 않아 Pending 상태이고, API KEY가 발급되지 않았습니다.

2주정도 기다리면 Riot에서 제 신청서와 제품을 확인한 뒤에 승인여부를 결정하고 알려준다고 합니다.

## Reference
* [DEVELOPER-RIOTGAMES](https://developer.riotgames.com/){:target="_blank"}
* [LoLog.me - 롤 전적검색 사이트](/lol/lolog-me/){:target="_blank"}
* [Riot API 사용법 - 계정 생성 & 소환사 정보 가져오기](/lol/riot-api/){:target="_blank"}