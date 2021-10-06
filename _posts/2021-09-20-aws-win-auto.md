---
title: "[AWS EC2 Windows] 재부팅 후 PM2 자동 실행 (사용자 데이터 스크립트)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-09-20T12:40:30+09:00
categories:
  - AWS
tags:
  - AWS
  - EC2
  - PM2
permalink: /aws/window-startup/
---
---
{% include ad-contents.html %}

리눅스의 경우 `pm2 save`와 `pm2 startup` 명령어를 사용해 재부팅시 pm2가 자동 실행되도록 할 수 있습니다.

그러나 pm2 startup 명령어는 아직 Unix 계열 운영체제에서만 작동합니다.

대신 윈도우 서버에서는 AWS EC2의 `사용자 데이터 스크립트` 기능을 사용하면 됩니다.
<!--more-->


## pm2 설치 경로 변경

`npm install pm2 -g` 명령어로 pm2를 설치했을 경우 pm2는 사용자 디렉토리에 저장됩니다.

사용자 데이터 스크립트로는 사용자 디렉토리에 접근할 수 없으므로 `npm -g`의 기본 경로를 변경해줍니다.

`npm config` 명령어로 npm의 기본 디렉토리를 접근하기 쉬운 경로로 변경해줍니다.

```
> npm config set prefix "C:\\npm"
> npm config set cache "C:\\npm-cache"
```

pm2를 다시 설치합니다. (사용자 디렉토리에 있는 모듈들을 제거하고 바꾼 위치에 다시 설치합니다)

```
npm install pm2 -g
```

이제 사용자 데이터 스크립트를 사용해 pm2를 실행시킬 수 있습니다.

## 사용자 데이터 스크립트 편집

![User Data Script](/assets/post-images/window-startup/user-data.png)

사용자 데이터 스크립트를 편집하기 위해서는 인스턴스가 중지된 상태여야합니다.

인스턴스를 중지하고, 인스턴스 설정 -> 사용자 데이터 편집 탭에 들어갑니다.

스크립트를 입력하고 저장합니다.

`<powershell>` 과 `</powershell>` 사이에 시작시 실행할 powershell 명령어를 입력합니다.

저의 경우 앵무새봇의 경로로 이동하고 pm2를 실행하는 스크립트를 작성하였습니다.

사용자 데이터 스크립트는 기본적으로 인스턴스를 처음 시작할때에만 실행됩니다.

마지막에 `<persist>true</persist>`를 입력해주면 재부팅을 할때마다 스크립트가 실행됩니다.

```
<powershell>
cd C:\Users\Administrator\parrot-bot-discord
pm2 start parrot-bot.js --time
</powershell>
<persist>true</persist>
```

{% include ad-contents.html %}

## EC2Launch script 실행

![User Data Script](/assets/post-images/window-startup/ec2-launch-setting.png)

사용자 데이터 스크립트 편집을 하고 인스턴스를 재시작하면 스크립트가 실행되지 않습니다.

EC2Launch script를 실행하여 사용자 데이터 스크립트가 실행되도록 해야합니다.

EC2 인스턴스에 접속하여 EC2 Launch Setting을 실행합니다.

EC2 Launch Setting의 Sysprep 탭에서 Shutdown with Sysprep 버튼을 눌러 인스턴스를 중단합니다. (약간의 시간이 소요됩니다)

인스턴스가 완전히 종료되면 인스턴스를 재시작합니다.

이번에는 사용자 데이터 스크립트가 정상적으로 실행됩니다.

스크립트에 `<persist>true</persist>`를 추가했다면 재부팅할 때마다 스크립트가 자동으로 실행됩니다.

## Reference

* [시작 시 Windows 인스턴스에서 명령 실행](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html){:target="_blank"}

* [npm 모듈의 전역 설치 위치 변경](https://soooprmx.com/npm-%EB%AA%A8%EB%93%88%EC%9D%98-%EC%A0%84%EC%97%AD-%EC%84%A4%EC%B9%98-%EC%9C%84%EC%B9%98-%EB%B3%80%EA%B2%BD/){:target="_blank"}

{% include ad-contents.html %}