---
title: "[Effective C#] 아이템 28: 확장 메서드를 이용하여 구체화된 제네릭 타입을 개선하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-06-01T15:57:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item28/
---
---

아이템 27과 비슷하게 List<int>, Dictionary<EmployeeID, Employee>와 같이 이미 구체화된 컬렉션 타입에 확장 메서드를 추가하여 새로운 기능을 추가할 수 있다.

<!--more-->

예시로, System.Linq.Enumerable 클래스는 특정 IEnumerable<T> 타입에 대한 확장 메서드들이 정의되어 있다.

```cs
public static class Enumerable
{
	public static int Average(this IEnumerable<int> sequnece);
	public static int Max(this IEnumerable<int> sequence);
	public static int Min(this IEnumerable<int> sequence);
	public static int Sum(this IEnumerable<int> sequence);

	// 다른 메서드 생략
}
```

### 확장 메서드를 사용했을 때의 장점
1. 단순한 기능을제공하는 메서드를 다양하게 재사용할 수 있다.
2. 컬렉션 고유의 저장소 모델과 무관하게 기능을 구현할 수 있다. (IEnumerable 등 사용)
