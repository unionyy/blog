---
title: "[PM2] Configuration File 로 프로세스 한번에 관리하기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-05T01:15:30+09:00
categories:
  - Node.js
tags:
  - Node.js
  - pm2
permalink: /nodejs/pm2-config/
---
---

PM2를 사용할 때에 Configuration File 을 생성해놓으면 여러 프로세스들을 한번에 실행할 수 있고, 옵션도 편리하게 추가할 수 있습니다.

<!--more-->
{% include ad-contents.html %}

## Configuration File 생성
pm2 명령어로 간단하게 Configuration File (ecosystem.config.js) 을 생성할 수 있습니다.
```
$ pm2 init simple
```

## ecosystem.config.js 작성
생성된 ecosystem.config.js 파일을 원하는 대로 수정해줍니다.

[앵무새봇](https://koreanbots.dev/bots/795333228662751253){:target="_blank"} 서버의 경우 아래와 같이 설정해주었습니다.
```js
module.exports = {
  apps : [{
        name   : "parrot0",
        script : "./parrot-bot-discord/parrot-bot.js",
        args: "2 0",
        time: true,
        max_memory_restart: "600M"
  },
  {
        name: "parrot1",
        script: "./parrot-bot-discord/parrot-bot.js",
        args: "2 1",
        time: true,
        max_memory_restart: "600M"
  }]
}
```

각 옵션에 대한 설명은 아래를 참고하세요.
* [[PM2 옵션] 앱 이름, 시간 로그, argv, 메모리 제한 (--name, --time, -- , --max-memory-restart)](/nodejs/pm2-options/){:target="_blank"}

## 실행하기
```bash
# Start all applications
$ pm2 start ecosystem.config.js

# Stop all
$ pm2 stop ecosystem.config.js

# Restart all
$ pm2 restart ecosystem.config.js

# Reload all
$ pm2 reload ecosystem.config.js

# Delete all
$ pm2 delete ecosystem.config.js
```

## Reference
* [Configuration File](https://pm2.keymetrics.io/docs/usage/application-declaration/){:target="_blank"}

{% include ad-contents.html %}