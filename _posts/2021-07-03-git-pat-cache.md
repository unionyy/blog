---
title: "[PAT] GitHub Personal Access Token(PAT) 발급 & 캐싱"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-07-03T13:10:30+09:00
categories:
  - GitHub
tags:
  - GitHub
  - PAT
  - AWS
  - EC2
  - Linux
permalink: /github/pat-cache/
---
---
AWS EC2 Linux 서버에서 GitHub id, password를 사용하여 `git pull`을 할 때마다 `[GitHub] Deprecation Notice`라는 제목의 메일이 왔습니다. id와 password를 통해 깃허브에 접근하는 것이 보안상의 이유로 곧 사용할 수 없게 될 것이라는 내용의 메일이었습니다. 그래서 서버에서 사용할 전용 Personal Access Token을 발급하고 이를 매번 입력할 필요가 없도록 캐싱해주었습니다.
<!--more-->

{% include ad-contents.html %}

## GitHub PAT 발급
* [Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token){:target="_blank"}

메뉴 -> Settings -> Developer settings -> Personal access token 의 경로에서 Generate new token 버튼을 눌러 토큰을 생성합니다.

토큰은 생성 당시에 한번만 노출되고 더이상 확인할 수 없으니 잘 저장해두셔야 합니다. 토큰을 잃어버릴 경우에는 찾을 수 없고, 재발급 받아야합니다.

## 캐싱
* [Caching your GitHub credentials in Git](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git){:target="_blank"}

리눅스 캐싱 명령어
```
$ git config --global credential.helper cache
# Set git to use the credential memory cache
```
기본적으로 15분 동안 캐싱이 되고, 캐싱 시간을 변경하기 위해서는 아래의 명령어를 사용합니다.
```
$ git config --global credential.helper 'cache --timeout=3600'
# Set the cache to timeout after 1 hour (setting is in seconds)
```

## Reference
* [Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token){:target="_blank"}

* [Caching your GitHub credentials in Git](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git){:target="_blank"}

{% include ad-contents.html %}