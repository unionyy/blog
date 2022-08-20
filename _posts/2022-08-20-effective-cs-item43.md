---
title: "[Effective C#] 아이템 43: 쿼리 결과의 의미를 명확히 강제하고, Single()과 First()를 사용하라"
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

LINQ 라이브러리는 시퀀스를 반환하지만 단일 요소를 반환하는 메서드도 있다.

<!--more-->

* Single(): 정확히 1개의 요소 반환, 쿼리 결과에 요소가 없거나 2개 이상일 경우에는 예외 발생
* SingleOrDefault(): 1개의 요소 또는 null을 반환, 쿼리 결과에 요소가 없을 경우 null을 반환하고 2개 이상일 경우 예외 발생
* First(): 첫번째 요소를 반환, 요소가 없을 경우 예외 발생
* FirstOrDefault(): 첫번째 요소나 null을 반환, 요소가 없을 경우 null을 반환

다음은 Single()의 사용 예시이다.

```cs
var somePeople = new List<Person>{
   new Person { FirstName = "Bill", LastName = "Gates"},
   new Person { FirstName = "Bill", LastName = "Wagner"},
   new Person { FirstName = "Bill", LastName = "Johnson"}};

// 시퀀스 내에 여러 개의 요소가 포함되므로
// 예외를 일으킨다.
var answer = (from p in somePeople
            where p.FirstName == "Bill"
            select p).Single();
```

Bill이 1명일 경우에는 FirstName이 Bill인 Person 객체를 반환하고,

그 외에는 예외를 발생시킨다.


다음은 First()의 사용 예시이다.

```cs
var answer = (from p in Forwards
            where p.GoalsScored > 0
            orderby p.GoalsScored
            select p).Skip(2).First();
```

위 코드는 세번째 요소를 반환한다.

First()를 호출하기 전에 Skip(2)를 호출하여 맨 앞의 두번째 요소를 생략하였기 때문이다.


위의 메서드들 처럼 단일 값을 가져올 때에는 단일 요소를 가진 시퀀스를 가져오기 보다는 스칼라 값 자체를 가져오도록 쿼리를 작성하는 것이 좋다.

그렇게 해야 코드를 이해하기도 쉽고 예외 상황에 대비하기도 쉽다.

## Reference
* [Enumerable.Single 메서드](https://docs.microsoft.com/ko-kr/dotnet/api/system.linq.enumerable.single?view=net-6.0){:target="_blank"}
