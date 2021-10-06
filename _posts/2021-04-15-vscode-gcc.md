---
title: "Visual Studio Code에서 GCC 사용하기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-04-15T18:04:30+09:00
categories:
  - Visual Studio Code
tags:
  - Visual Studio Code
  - GCC
permalink: /vscode/gcc/
---
---
{% include ad-contents.html %}


리눅스 환경에서는 터미널에서 GCC를 쉽게 설치하고 사용할 수 있습니다. 그러나 윈도우에서는 다소 번거로운 작업이 필요합니다. 윈도우 cmd(명령 프롬프트)에서 GCC를 사용할 수 있도록 세팅을 하고 VS code에서 f5키를 눌렀을 때 디버그가 실행되도록 설정해봅시다.

<!--more-->


## Windows 환경에서 GCC 설치 (MinGW)

### 1. MinGW-w64 다운로드

* [MinGW-w64 - for 32 and 64 bit Windows](https://sourceforge.net/projects/mingw-w64/files/){:target="_blank"}

위의 링크에서 **x86_64-posix-seh** 파일(.7z)을 다운받습니다.

![download](/assets/post-images/vscode-gcc/download.png)

installer로 설치할 경우 제대로 설치가 되지 않는 문제가 발생하는 경우가 있어서 압축파일을 받는 것을 추천합니다.

### 2. 압축 해제

다운받은 .7z 파일의 압축을 해제하고 원하는 위치에 저장해줍니다.

저는 `C:\Program Files\mingw64` 이 위치에 저장했습니다.

(.7z 파일의 압축 해제가 되지 않을 경우 [반디집](https://kr.bandisoft.com/bandizip/){:target="_blank"}을 설치하시면 됩니다)

### 3. 환경 변수 설정

명령 프롬프트의 어느 위치에서나 GCC를 실행시킬 수 있도록 환경 변수(Path)를 설정해줘야합니다.

![path](/assets/post-images/vscode-gcc/path.png)

시작버튼을 누르고 `환경` 검색 -> `시스템 환경 변수 편집` 에서 다음과 같이 설정해줍니다.

![path2](/assets/post-images/vscode-gcc/path2.png)

사용자 변수의 Path에 MinGW의 bin 폴더를 추가해주는 작업입니다.

### 4. 확인

명령 프롬프트를 실행해서 `gcc --version`을 입력하여 gcc가 제대로 설치 되었는지 확인합니다.

{% include ad-contents.html %}

## Visual Studio Code 에서 GCC로 컴파일하기

GCC를 설치했다면 VS Code의 터미널(`ctrl + j`)에서 명령어를 입력하여 gcc를 실행시킬 수 있습니다. 예) `g++ file_name.cpp`

그러나 몇가지 설정을 하면 f5키를 누르는 것 만으로 컴파일이 되도록 설정할 수 있습니다.

### 1. C/C++ 익스텐션 설치

먼저 VS Code의 C/C++ 익스텐션을 설치해줍니다.

### 2. 디버그 설정

* 컴파일하려는 파일을 열고 `f5`를 눌러줍니다.

* `Select environment` 창이 뜨면 C++(GDB/LLDB)를 선택해줍니다.

* 구성으로 g++.exe를 선택해줍니다.

이제 터미널이 자동으로 열리고 디버그 및 실행이 진행됩니다. 설정은 프로젝트마다 한번만 해주면 됩니다.

## Reference

* [Using GCC with MinGW](https://code.visualstudio.com/docs/cpp/config-mingw){:target="_blank"}

* [MinGW-w64 - for 32 and 64 bit Windows](https://sourceforge.net/projects/mingw-w64/files/){:target="_blank"}

* [반디집](https://kr.bandisoft.com/bandizip/){:target="_blank"}

{% include ad-contents.html %}
