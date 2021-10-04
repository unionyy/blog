---
title: "[PM2 옵션] 앱 이름, 시간 로그, argv, 메모리 제한 (`--name`, `--time`, `--` , `--max-memory-restart`)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-05T00:52:30+09:00
categories:
  - Node.js
tags:
  - Node.js
  - pm2
permalink: /nodejs/pm2-options/
---
---

[앵무새봇](https://koreanbots.dev/bots/795333228662751253){:target="_blank"}을 배포할 때 사용하는 PM2 옵션입니다.

<!--more-->
{% include ad-contents.html %}

## `--time`
로그가 찍힐 때 시간도 함께 찍히도록 합니다.

## `--name myApp`
앱을 실행할 때에 이름을 붙여줍니다. 같은 스크립트를 여러개 실행해야할 때 유용합니다. (각각 다른 이름 붙이기)

## `-- myArg1 myArg2`
앱을 실행할 때에 매개변수를 줄 수 있습니다. 스크립트에서 접근할 때에는 `process.argv[]` 로 접근합니다. (`process.argv[2] = myAgr1`)

## `--max-memory-restart 100M`
앱의 메모리 상한선을 설정합니다. 메모리가 상한선을 초과할 경우 앱이 자동으로 재시작됩니다.

## 예제
```bash
$ pm2 start parrot-bot.js --time --name parrot0 -- 0 2 --max-memory-restart 800M
```

## ecosystem.config.json 에서 사용
* [[PM2] Configuration File 로 프로세스 한번에 관리하기](/nodejs/pm2-config/){:target="_blank"}

## Reference
* [PM2 Process Management Quick Start](https://pm2.keymetrics.io/docs/usage/quick-start/){:target="_blank"}
* [process.argv](https://nodejs.org/docs/latest/api/process.html#process_process_argv){:target="_blank"}

{% include ad-contents.html %}