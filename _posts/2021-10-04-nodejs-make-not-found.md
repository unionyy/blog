---
title: "[npm 오류] Error: not found: make"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-04T16:40:30+09:00
categories:
  - Node.js
tags:
  - Node.js
  - npm
permalink: /nodejs/make-not-found/
---
---
{% include ad-contents.html %}

[앵무새봇](https://koreanbots.dev/bots/795333228662751253){:target="_blank"}의 서버를 증설하기 위해 새로운 AWS EC2 인스턴스를 생성하였습니다.

서버에 nvm과 Node.js를 설치하고 프로젝트를 클론한 뒤에 **npm install** 명령어를 실행하였으나 **Error: not found: make** 오류가 뜨면서 모듈 하나가 설치되지 않았습니다.

<!--more-->

## 해결 방법
**build-essential**을 설치하면 해결됩니다.

```
$ sudo apt-get install build-essential
```

## Reference
* [npm failed to install time with make not found error](https://stackoverflow.com/questions/14772508/npm-failed-to-install-time-with-make-not-found-error){:target="_blank"}
* [자습서: Amazon EC2 인스턴스에서 Node.js 설정](https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html){:target="_blank"}

{% include ad-contents.html %}