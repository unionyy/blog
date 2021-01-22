---
title: "Visual Studio Code에서 Markdown(.md) 미리보기"
excerpt_separator: "<!--more-->"
layout: single
categories:
  - Coding
  - Visual Studio Code
tags:
  - vscode
  - markdown
  - github
---

GitHub에 REAMDME 파일을 작성할 때

Jekyll을 이용하여 GitHub Page를 만들 때

Markdown(.md)을 사용합니다.

이를 작성과 동시에 미리보고자 합니다.
<!--more-->
## 단축키 `ctrl+k` `v`
Markdown 파일이 열린 상태에서

`ctrl+k`를 누르고 1초 내에 `v`를 누르면

화면이 분할되고 오른쪽에 미리보기 창이 열립니다.
![shortcut](/assets/post-images/markdown0.PNG)

## GitHub 스타일 마크다운 적용
[이곳](https://github.com/aliencube/markdownpad-github){:target="_blank"}에서 markdownpad-github.css 파일을 받아 프로젝트 폴더 안에 넣어줍니다.

그리고 Settings > Extensions > Markdown에서

다음과 같이 markdownpad-github.css를 추가해줍니다.
![setting](/assets/post-images/markdown1.PNG)

GitHub Style Mardown 미리보기 적용 완료.
![githubstyle](/assets/post-images/markdown2.PNG)


## Reference
* [Visual Studio Code 에서 깃헙 스타일 마크다운 사용하기](https://blog.aliencube.org/ko/2016/07/06/markdown-in-visual-studio-code/){:target="_blank"}
* [Could not load 'markdown.styles': *.css](https://github.com/microsoft/vscode/issues/77290){:target="_blank"}
* [GitHub/aliencube/markdownpad-github](https://github.com/aliencube/markdownpad-github){:target="_blank"}