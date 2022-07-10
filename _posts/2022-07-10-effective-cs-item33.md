---
title: "[Effective C#] 아이템 33: 필요한 시점에 필요한 요소를 생성하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-10T15:47:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item33/
---
---

yield return을 사용해서 시퀀스를 생성하면 사용되지 않을 요소를 미리 생성하는 것을 피할 수 있다.

<!--more-->

다음 예시 코드는 정수값 시퀀스를 생성하는 메서드이다.
```cs
static IList<int> CreateSequence(int numberOfElements, int startAt, int stepBy)
{
	var collection = new List<int>(numberOfElement);
	for (int i = 0; i < numberOfElements; i++)
		collection.Add(startAt + i * stepBy);

	return collection;
}
```
위 코드는 항상 numberOfElements 개의 요소를 생성한다.

이를 중간에 멈출 방법은 없다.


다음과 같이 yield return을 사용하면 요소를 필요할 때에 생성한다.
```cs
static IEnumerable<int> CreateSequence(int numberOfElements, int startAt, int stepBy)
{
	for (var i = 0; i < numberOfElements; i++)
		yield return startAt + i * stepBy;
}
```
위의 코드는 각 요소가 필요할 때에만 생성된다.

따라서 중간에 멈추는 것도 가능하다.
```cs
// 요소가 1000보다 같거나 커지면 요소 생성이 멈춤
var sequence = CreateSequnece(10000, 0, 7).
	TakeWhile((num) => num < 1000);
```

이처럼 필요한 시점에 요소를 생성하도록 하면 불필요한 비용을 줄일 수 있다.

## Reference
* [Enumerable.TakeWhile 메서드](https://docs.microsoft.com/ko-kr/dotnet/api/system.linq.enumerable.takewhile?view=net-6.0){:target="_blank"}
