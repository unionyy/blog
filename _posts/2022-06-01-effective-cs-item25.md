---
title: "[Effective C#] 아이템 25: 타입 매개변수로 인스턴스 필드를 만들 필요가 없다면 제네릭 메서드를 정의하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-06-01T15:47:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item25/
---
---

## 제네릭 클래스를 사용하는 대신 제네릭 메서드를 사용했을 때의 장점
1. 각 메서드별로 제약 조건을 설정할 수 있다.
2. 타입별로 특화된 메서드를 정의할 수 있다.
3. 매개변수를 명시적으로 지정할 필요가 없다.

<!--more-->

```cs
public static class Utils<T>
{
	public static T Max(T left, T right) =>
		Comparer<T>.Default.Compare(left, right) < 0 ? right : left;

	public static T Min(T left, T right) =>
		Comparer<T>.Default.Compare(left, right) < 0 ? left : right;
}
```
### 위 클래스를 사용할 때의 문제점
1. 사용할 때마다 타입 매개변수를 지정해줘야 한다
2. Max, Min 메서드를 이미 갖고 있는 타입들이 많지만 모두 Compare<T>를 사용한다.


```cs
public static class Utils
{
	public static T Max<T>(T left, T right) =>
  		Comparer<T>.Default.Compare(left, right) < 0 ? right : left;

	public static double Max(double left, double right) =>
		Math.Max(left, right)
	
	// 다른 타입에 대한 Max 메서드 생략

 	public static T Min<T>(T left, T right) =>
		Comparer<T>.Default.Compare(left, right) < 0 ? left : right;

	// 다른 타입에 대한 Min 메서드 생략
}
```

Utils 클래스를 일반 클래스로 바꾸고 Min, Max 메서드를 제네릭으로 바꿨다.

이렇게 되면 메서드를 사용할 때에 타입 매개변수를 명시할 필요가 없어진다.

또한 특정한 타입들에 대해 오버로딩을 함으로써 특화된 메서드를 정의할 수 있고, 효율적으로 동작하는 코드를 작성할 수 있다.

### 제네릭 클래스를 사용해야 하는 경우
1. 클래스 내에 타입 매개변수로 주어진 타입으로 내부 상태를 유지해아 하는 경우 (ex, 컬렉션)
2. 제네릭 인터페이스를 구현하는 클래스를 만들어야 하는 경우

위 두가지 경우를 제외하고는 제네릭 클래스보다는 제네릭 메서드를 사용하는 것이 좋다.
