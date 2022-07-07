---
title: "[Effective C#] 아이템 30: 루프보다 쿼리 구문이 낫다"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-07T22:17:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item30/
---
---

### 쿼리 구문이나 메서드 호출 방식을 사용했을 때의 장점
* 가독성이 높아진다
* 개별 작업의 수행 시기를 연기할 수 있다

<!--more-->

다음 코드는 0 ~ 99의 (X, Y) 좌표 객체를 생성하고 X + Y < 100 인 좌표만을 골라 (0, 0)에서 가까운 순으로 정렬하는 코드이다. 
```cs
private static IEnumerable<Tuple<int, int>> ProduceIndices()
{
	var storage = new List<Tuple<int, int>>();

	for (var x = 0; x < 100; x++)
		for (var y = 0; y < 100; y++)
			if (x + y < 100)
				storage.Add(Tuple.Create(x, y));

	storage.Sort((point1, point2) =>
		(point2.Item1 * point2.Item1 + point2.Item2 * point2.Item2).CompareTo)
		point1.Item1 * point1.Item1 + point1.Item2 * point1.Item2));
	return storage;
}
```
반복문을 이용하면 연산이 복잡해질 수록 depth가 높아지고 가독성이 떨어진다.

또한 위의 코드에서는 Select 작업과 Sort 작업을 각각 따로 수행하여,

루프를 두번 순회하고 storage라는 별도의 메모리도 사용했다.


위와 동일한 작업을 하는 코드를 쿼리 구문을 사용하도록 변경하였다.
```cs
private static IEnumerable<Tuple<int, int>> QueryIndices()
{
	return from x in Enumerable.Range(0, 100)
			from y in Enumerable.Range(0, 100)
			where x + y < 100
			orderby (x * x + y * y) descending
			select Tuple.Create(x, y);
}
```
이처럼 쿼리 구문을 사용하면 코드가 깔끔해져서 가독성이 높아진다.

또한 쿼리 구문을 사용하면 여러 구문들을 다양하게 조합하여 한번의 순회 과정 동안에 여러 작업들을 한번에 수행할 수 있다.

때문에 for문을 사용한 코드와 달리 임시 메모리를 사용하지 않았고 루프를 두번 순회하지도 않았다.

## Reference
* [LINQ의 쿼리 구문 및 메서드 구문(C#)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq){:target="_blank"}
