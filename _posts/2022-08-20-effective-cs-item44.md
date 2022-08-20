---
title: "[Effective C#] 아이템 44: 바인딩된 변수는 수정하지 말라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-08-20T12:41:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item43/
---
---

클로저에서 캡처된 변수를 수정하면 예상치 못한 문제가 발생할 수 있다.

<!--more-->
```ca
var index = 0;
Func<IEnumerable<int>> sequence = () => Utilities.Generate(30, () => index++);

index = 20;

// 20부터 50까지 출력된다.
foreach (int n in sequence())
    WriteLine(n);

index = 100;

// 100 부터 30까지 출력된다.
foreach (var n in sequence())
    WriteLine(n);
```

아이템 41에서 클로저는 숨겨진 중첩 클래스를 생성한다고 했었다.

위 코드에서 index는 중첩 클래스의 멤버 변수로 대체된다.

또한 람다 표현식은 중첩 클래스 내의 메서드 내부에 포함된다.


따라서 바인딩된 변수를 수정할 경우 클로저로 해당 변수를 사용하는 모든 로직에 영향을 주게 되고,

이는 예상하기 어려운 동작을 야기한다.

따라서 바인딩된 변수는 수정하지 않는 것이 좋다.
