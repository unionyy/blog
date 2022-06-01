---
title: "[Effective C#] 아이템 26: 제네릭 인터페이스와 논제네릭 인터페이스를 함께 구현하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-06-01T15:51:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item26/
---
---

새로운 라이브러리를 개발할 때에 제네릭 타입뿐 아니라 고전적인 방식도 함께 지원하면 라이브러리의 활용도를 좀 더 높일 수 있다.

<!--more-->

제네릭이 아닌 방식도 지원하려면 다음 세가지에 대해서 논제네릭 방식을 지원해야한다.

1. 클래스와 인터페이스
2. public 속성
3. serialize 대상이 되는 요소


## Equals() 메서드 내에서 IEquatable<T>.Equals()를 호출하도록 하기

IEquatable<T> 를 구현하였을 때에 Object.Equals() 메서드에서 IEquatable<T>.Equals()를 호출하도록 할 수 있다.
```cs
// class MyClass : IEquatable<MyClass>

public override bool Equals(object obj)
{
	if (obj.GetType() == typeof(MyClass))
		return this.Equals(obj as MyClass);
	else
		return false;
}
```
위 코드의 5번째 줄에서 obj의 타입을 체크하는 이유는 obj가 Name을 상속한 파생 클래스일 경우에 타입이 다른데도 true가 될 수 있기 때문이다.

주의) Equals() 메서드를 override 했을 경우에는 GetHashCode() 메서드도 override 해야한다.

```cs
IEquatable<T>를 구현했다면 operator==와 operator!=도 함께 구현 하기
// class MyClass : IEquatable<MyClass>

public static bool operator ==(MyClass left, MyClass right)
{
	if (left == null)
		return right == null;
	return left.Equals(right);
}

public static bool operator !=(Name left, Name right)
{
	if (left == null)
		return right != null;
	return !left.Equals(right);
}

```

## IComparable<T>를 구현했다면 IComparable와 operator도 함께 구현하기

마찬가지로 선후관계를 확인하기 위해 IComparable<T>를 구현했다면,

제네릭이 아닌 IComparable과 operator(<, >, <=, >=)도 함께 구현하는 것이 좋다.

이에 대한 내용은 위의 item20에서 자세히 다뤘다.

## Reference
* [C# 의 타입 - 4. 객체의 식별 (Equals, GetHashCode)](https://tsyang.tistory.com/11){:target="_blank"}
