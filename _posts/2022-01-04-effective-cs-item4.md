---
title: "[Effective C#] 아이템 4: string.Format()을 보간 문열로 대체하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-01-04T09:40:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item4/
---
---
string.Format() 대신 보간 문자열을 사용하면 코드 가독성과 성능 면에서 이점이 있다.
<!--more-->

```cs
List<string> strs = new List<string>();

for(var i = 0; i < 10000000; i++) {

	// string.Format()
  strs.Add(String.Format("{0} + {1} = {2}", i, i + 1, 2 * i + 1));

  // 보간 문자열
  strs.Add($"{i} + {(i + 1)} = {(2 * i + 1)}");

  // ToString()
  strs.Add($"{i.ToString()} + {(i + 1).ToString()} = {(2 * i + 1).ToString()}");
}
```
## 코드 가독성 향상
string.Format() 방식에서는 문자열과 인자들이 분리되어 있어 가독성이 떨어진다.

문자열 보간을 이용하면 앞에서부터 한번에 훑을 수 있는 코드가 되어 가독성이 대폭 향상된다.

## 성능 향상
Effective C# 3판에서는 보간 문자열을 사용하면 object 타입으로의 박싱이 이루어진다고 나와있다. (보간 문자열을 사용하면 내부적으로는 String.Format()이 호출되기 때문)

그래서 박싱이 많이 일어날 경우에는 위의 ToString() 예시와 같이 값 타입의 변수를 미리 문자열로 대체하는 것이 성능상의 이점이 있다고 한다.

이는 C# 10.0 이전 까지는 맞는 말이다.

C# 10.0 부터는 DefaultInterpolatedStringHandler 형식이 추가되어, StringBuilder 형식으로 문자열이 생성된다.(https://devblogs.microsoft.com/dotnet/string-interpolation-in-c-10-and-net-6/)

때문에 이제 보간 문자열은 string.Format() 보다 뛰어난 성능을 보여준다.

책에서 나와있는 것 처럼 ToString()을 사용하여 변수 대신 문자열을 전달하는 방식을 사용하면 오히려 성능이 저하된다.


| |C# 9.0|C# 10.0|
|:------------|:---:|:---:|
|String.Format()|5.7s|5.9s|
|String.Format() + ToString()|4.8s|4.4s|
|보간 문자열|5.9s|2.1s|
|보간 문자열 + ToString()|4.4s|4.2s|

## Reference
* [문자열 보간](https://docs.microsoft.com/ko-kr/dotnet/csharp/tutorials/string-interpolation){:target="_blank"}
* [String Interpolation in C# 10 and .Net 6](https://devblogs.microsoft.com/dotnet/string-interpolation-in-c-10-and-net-6/){:target="_blank"}
* [Boxing 및 Unboxing](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/types/boxing-and-unboxing)){:target="_blank"}
* [성능](https://docs.microsoft.com/ko-kr/dotnet/framework/performance/performance-tips){:target="_blank"}
