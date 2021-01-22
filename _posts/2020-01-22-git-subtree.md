---
title: "[subtree] GitHub Repository 합치기"
layout: single
categories:
  - Coding
  - GitHub
tags:
  - github
  - subtree
  - repository
  - commit log
---
## 문제점
코딩 실습들을 개별 Repository에서 진행하면서 문제가 생겼습니다.
* 실습용 repository들이 쌓여서 지저분해보인다.
* fork해와서 진행한 실습들은 Contribution Graph에 표시되지 않는다.

## 해결방안
### subtree
`git subtree add`를 이용하면,

하나의 repository 안에 다른 repository를 그대로 복사해올 수 있습니다.

이때, 기존의 commit log를 함께 복사해올 수 있습니다!

```
git subtree add --prefix=(해당 Repository 하위의 디렉터리 구조) (옮겨올 Repository 주소) (옮겨올 Repository의 branch) 
```

저의 경우,
```
git subtree add --prefix=nodejs-practice https://github.com/unionyy/nodejs-practice main
```

기존 repository의 commit log를 없애려면 `--squash` 옵션을 마지막에 추가하면 됩니다.

## 결과
하나의 repository의 하위 디렉토리 안에 각각의 repository가 복사되었습니다.
![success](https://blog.uniony.me/assets/post-images/subtree0.PNG)

기존의 commit log들도 성공적으로 옮겨졌습니다.
![commit](https://blog.uniony.me/assets/post-images/subtree1.PNG)

## 단점
각각의 파일의 commit history를 확인하기 어려워집니다.
![bad](https://blog.uniony.me/assets/post-images/subtree2.PNG)

개별 파일 또는 디렉토리의 commit history가 제대로 보여지지 않습니다.

repository를 복사할 때 만들어진 commit 하나만 보여집니다.

찾을 내용이 생기면 repository 전체 히스토리를 참고해야 할 듯 합니다.

## Reference
* [Git Repository 합치기 (commit log 유지) - subtree 이용](http://yeoseon.kr/git-repository-habcigi-commit-log-yuji-subtree-iyong/)
* [Git subtree: the alternative to Git submodule](https://www.atlassian.com/git/tutorials/git-subtree)