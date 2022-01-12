---
title: "[Effective C#] 아이템 5: 문화권별로 다른 문자열을 생성하려면 FormattableString을 사용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-01-12T08:54:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item5/
---
---
보간 문자열은 여러 형식으로 암시적 형변환이 가능하다.

FormattableString을 사용할 경우 여러 문화권에 대응할 수 있다.
<!--more-->

```cs
//string (로컬 기준으로 생성됨)
string str = $"{DateTime.Now.Hour}:{DateTime.Now.Minute}";

//FormattableString (여러 포멧으로 변경할 수 있음)
FormattableString fStr = $"{DateTime.Now.Hour}:{DateTime.Now.Minute}";

//string
var vStr = $"{DateTime.Now.Hour}:{DateTime.Now.Minute}";
```
### 보간 문자열의 암시적 형변환
* string
* FormattableString
* IFormattble

FormattableString을 사용할 경우 여러 문화권에 대응할 수 있다.

(문자열을 원하는 포멧으로 변경할 수 있다)

var로 보간 문자열을 선언할 경우 string이 되므로, FormattableString이 필요할 경우 변수를 명시적으로 선언해주어야한다.

(책에서는 FormattableString을 반환하는 경우도 있다고 했지만, 테스트 결과 String만을 반환하였다)
