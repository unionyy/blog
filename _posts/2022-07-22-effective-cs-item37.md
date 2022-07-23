---
title: "[Effective C#] 아이템 37: 쿼리를 사용할 때는 즉시 평가보다 지연 평가가 낫다"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-22T17:35:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item37/
---
---

쿼리를 정의하면 결과 데이터나 시퀀스를 즉각적으로 얻어오는 것은 아니다.
실제로 쿼리의 결과를 이용하여 순회를 수행해야만 결과가 생성된다.
이를 지연 평가 (Lazy Evaluation)이라고 한다.

<!--more-->

반대로 즉각적으로 결과를 얻어와야 하는 경우는 즉시 평가 (Eager Evaluation) 이라고 한다.

다음은 지연 평가에 대한 예시 코드이다.
```cs
// Main
var answers = from number in AllNumbers()
    select number;

var smallNumbers = answers.Take(10);
foreach (var num in smallNumbers)
    Console.WriteLine(num);

// 무한 시퀀스
static IEnumerable<int> AllNumbers()
{
    var number = 0;
    while (number < int.MaxValue)
    {
        yield return number++;
    }
}
```
위 코드를 실행하면 0부터 9까지 정수가 출력되고 프로세스가 종료된다.
Main 메서드의 쿼리문에서 무한 시퀀스를 호출했으나 시퀀스 전체가 호출되지는 않았던 것이다.

반면 다음 코드는 무한이 수행된다.
```cs
// 무한 수행
var answers2 = from number in AllNumbers()
    where number < 10
    select number;

foreach (var num in answers2)
    Console.WriteLine(num);
```
where 절은 반드시 모든 시퀀스를 순회해야 하는 구문이기 때문이다.
마찬가지로 orderby, Max, Min 또한 전체 시퀀스가 필요하다.

무한 시퀀스가 아닐 경우에도 시퀀스를 필터링하는 쿼리 메서드는 다른 쿼리 메서드 보다 먼저 수행하는 것이 좋다.
필터링을 통해 시퀀스가 줄어들면 이후에 실행되는 쿼리의 성능이 개선되기 때문이다.

위의 예시들과 같이 쿼리에서는 기본적으로 지연 평가가 사용되고 대부분의 경우 지연 평가가 더 나은 접근 방식이다.

그러나 간혹 특정 시점에 값을 받드시 알아야 하는 경우가 있을 수 있다.
이 때에는 ToList()나 ToArray() 메서드를 이용하면 된다.
이 두 메서드는 쿼리가 즉각 수행되게 하고 현재의 스냅샷을 저장한다.
