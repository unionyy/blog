---
title: "[Effective C#] 아이템 32: Action, Predicate, Function과 순회 방식을 분리하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-10T15:36:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item32/
---
---

이터레이터 메서드의 사용 방식에는 크게 두가지 유형이 있다.

1. 시퀀스 내의 개별 항목을 이용하여 작업을 수행하는 유형 (Action, Predicate, Function)
2. 시퀀스의 순회 방식에 변경을 주는 유형

<!--more-->

아이템 31에서 예시로 들었던 Unique<T> 메서드는 시퀀스 순회 방식에 변경을 주는 유형이었다.

시퀀스 내의 개별 항목을 이용하여 작업을 수행하는 유형에는 델리게이트가 사용될 수 있다.

자주 사용되는 3가지 유형의 델리게이트는 .NET 라이브러리 내에 포함되어 있다.
```cs
namespace System
{
	public delegate bool Predicate<T>(T obj);
	public delegate void Action<T>(T obj);
	public delegate TResult Func<T, TResult>(T arg);
}
```


다음은 델리게이트를 사용하는 시퀀스 메서드의 사용 예시 코드이다.
```cs
// List<T>.RemoveAll() 메서드는 Perdicate를 사용한다. (값이 5인 멤버를 모두 제거)
myInts.RemoveAll(collectionMember => collectionMember == 5);

// List<T>.ForEach() 메서드는 Action을 사용한다. (모든 멤버를 각각 출력)
myInts.ForEach(collectionMemeber => WriteLine(collectionMember));
```



다음은 시퀀스 내의 개별 항목을 이용하여 작업을 수행하는 유형의 예이다.
```cs
// Predicate<T>를 사용한 필터 메서드
public static IEnumerable<T> Where<T>(
	IEnumerable<T> sequence,
	Predicate<T> filterFunc)
{
	foreach (T item in sequence)
		if (filterFunc(item))
			yield return item;
}

// Func<T>를 사용하여 새로운 시퀀스를 생성하는 메서드
public static IEnumerable<T> Select<T>(
	IEnumerable<T> sequence, Func<T, T> method)
{
	foreach (T element in sequence)
		yield return method(element);
}
```


위에서 설명했던 것처럼 (1) 시퀀스를 순회하는 것과 (2) 시퀀스의 개별 요소에 대해 작업을 수행하는 것을 구분해야 한다.

시퀀스의 개별 요소에 대해 작업을 수행할 때에는 델리게이트를 이용할 수 있다.
