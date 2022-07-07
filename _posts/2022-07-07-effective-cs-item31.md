---
title: "[Effective C#] 아이템 31: 시퀀스에 사용할 수 있는 조합 가능한 API를 작성하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-07T22:22:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item31/
---
---

시퀀스를 다루는 메서드는 C# 이터레이터를 활용하여 작성할 수 있으며 결과가 필요한 시점에 메서드가 수행되도록 할 수 있다.

<!--more-->

이터레이터 메서드는 IEnumerable<T>(단일 시퀀스)를 입력으로 취하고 그 결과로도 IEnumerable<T>를 반환하는 메서드이다.

이터레이터 메서드를 작성할 때에 yield return을 사용하면 메서드 내의 개별 요소를 저장하기 위해 별도로 저장소를 마련할 필요가 없다.

(정확히 값이 필요한 시점에 다음 요소를 가져온다)

### 이터레이터 메서드를 사용했을 때의 장점
* 재사용성이 높아진다. (다양한 방식으로 조합 가능)
* 여러 메서드를 하나로 조합해서 사용하면 런타임 효율이 개선된다. (전체 시퀀스를 한번만 순회)
* 추가 저장소가 필요 없다. (지연 수행 모델)
* 제네릭 메서드로 변경하기에 용이하여 재사용성이 더욱 좋아진다.


다음 메서드는 정수 배열을 받아와서 중복값을 제거한 후 각기 한 번만 값을 출력하는 코드이다.
```cs
public static void Unique(IEnumerable<int> nums)
{
	var uniqueVals = new HashSet<int>();
	
	foreach (var num in nums)
	{
		if (!uniqueVals.Contains(num))
		{
			uniqueVals.Add(num);
			WriteLine(num);
		}
	}
}
```
위 코드에서 중복값을 제거하는 부분을 재사용할 수 있도록 변경하면,
```cs
// 중복값 제거 메서드
public static IEnumerable<int> Unique(IEnumerable<int> nums)
{
	var uniqueVals = new HashSet<int>();

	foreach (var num in nums)
	{
		if (!uniqueVals.Contains(num))
		{
			uniqueVals.Add(num);
			yield return num;
		}
	}
}

// 사용
foreach (var num in Unique(nums))
	WriteLine(num);
```
yield return은 각 요소가 필요할 때 호출된다.

따라서 시퀀스 내의 개별 요소에 대하여 지연 평가/수행이 가능하다.

또한 메서드가 foreach 루프를 포함하고 있어도 조합 가능한 메서드로 만들 수 있다.


제네릭 메서드로 변경하기도 쉽다.
```cs
public static IEnumerable<T> Unique<T>(IEnumerable<T> sequnece)
{
	var uniqueVals = new HashSet<T>();

	foreach (var num in nums)
	{
		if (!uniqueVals.Contains(num))
		{
			uniqueVals.Add(num);
			yield return num;
		}
	}
}
cs
제네릭 메서드는 일반 메서드보다 재사용성이 좋다.


제곱 메서드를 추가로 작성하여 조합해보자
```cs
// 제곱 메서드
public static IEnumerable<int> Square(IEnumerable<int> nums)
{
	foreach (var num in nums)
		yield return num * num;
}

// 조합하여 사용
foreach (var num in Square(Unique(nums)))
	WriteLine("Numer returned from Unique: {0}", num);
```
이처럼 이터레이터 메서드를 조합해서 사용하면 한번의 순회에 작업이 수행된다.

각각의 개별 요소 단위로 메서드 조합이 순차적으로 수행되는 것이다.

![4-1](/assets/post-images/effective-cs-item31/4-1.png)

이처럼 이터레이터 메서드들을 만들어두면 다단계 작업 과정을 거쳐야만 결과를 만들어낼 수 있는 알고리즘을 쉽고 효과적으로 작성할 수 있다.
