---
title: "[Effective C#] 아이템 6: nameof() 연산자를 적극 활용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-01-12T09:08:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item6/
---
---
nameof()는 타입, 변수, 인터페이스, 네임스페이스의 심볼을 문자열로 대체해준다.
<!--more-->

nameof()를 통해 문자열을 만들면 타입 정보를 잃지 않아서 디버깅에 용이하다.

또한 이름을 변경해야할 때 실수를 줄이고 오류를 발견하기 쉽다.

nameof()는 항상 로컬 이름을 반환한다.

```cs
var id = nameof(System.IO);
// "System.IO"가 아니라 "IO" 반환 
```

## Reference
* [nameof 식](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/nameof){:target="_blank"}
