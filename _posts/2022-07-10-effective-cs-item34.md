---
title: "[Effective C#] 아이템 34: 함수를 매개변수로 사용하여 결합도를 낮추라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-10T15:50:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item34/
---
---

컴포넌트 간의 계약 관계를 기술할 때에 베이스 클래스나 인터페이스를 사용한다.

또는 함수를 매개변수로 취하는 방식을 활용할 수도 있다.

<!--more-->

## 베이스 클래스를 상속하도록 했을 때
### 장점
* 사용자가 쉽게 사용할 수 있다.
* 계약 사항이 명확하다.
* 자주 사용되는 기능은 미리 구현해둘 수도 있다.
* 추상 메서드의 구현이 보장되어 있다. (컴파일러가 체크)
### 단점
* 제약이 많다.
* 사용자 입장에서 매우 제한적이다.

## 인터페이스를 작성하고 구현하도록 했을 때
베이스 클래스에 의존하는 방식보다는 조금 느슨한 결합을 만들 수 있다.
### 장점
* 사용자에게 클래스 계층 구조를 강제하지 않는다.
### 단점
* 재사용 가능 코드를 제공하기 어렵다.

## 델리게이트를 활용했을 때
인터페이스를 사용했을 때보다도 느슨한 결합을 구성할 수 있다.


아이템 32에서 살펴보았던 List.RemoveAll() 메서드는 델리게이트를 활용하는 메서드였다.
```cs
void List<T>.RemoveAll(Predicate<T> match);
```


RemoveAll() 메서드는 델리게이트가 아니라 인터페이스를 사용할 수도 있다.
```cs
// 추가적으로 부적절한 연관 관계가 생김
public interface IPredicate<T>
{
	bool Match(T soughtObject);
}

public class List<T>
{
	public void RemoveAll (IPredicate<T> match)
	{
		// 생략
	}

	// 생략
}

// 이 인터페이스를 사용하려면 추가적인 작업이 필요하다.
public class MyPredicate : IPredicate<int>
{
	public bool Match(int target) =>
		target < 100;
}
```
델리게이트를 사용하여 RemoveAll() 메서드를 수행하는 것이 훨씬 간편하다.



IComparable<T> 나 IEquatable<T> 와 같은 인터페이스는 타입의 선후관계 혹은 동일성을 지원한다는 것을 명확히 드러낸다.

그러나 IPredicate<T>와 같은 인터페이스는 이 타입이 무엇을 지원하는지 명확하지 않다.


이처럼 인터페이스나 베이스 클래스를 만들려고 하는 경우에는 델리게이트를 활용하는 방법도 검토해 보는 것이 좋다.

### 주의할 점
델리게이트로 null이 전달되었을 경우를 대비해야 한다.
