---
title: "[Effective C#] 아이템 24: 베이스 클래스나 인터페이스에 대해서 제네릭을 특화하지 말라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-06-01T15:45:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item24/
---
---

베이스 클래스나 인터페이스로 제네릭 메서드를 오버로딩할 경우, 명시적 형변환을 해줘야만 선택된다.

<!--more-->

```cs
static void MyMethod<T>(T obj)
{
	// 생략
}

static void MyMethod(MyBase b)
{
	// 생략
}

static void MyMethod(IMyInterface obj)
{
	// 생략
}

위는 MyMethod<T>() 메서드가 베이스 클래스나 인터페이스로 오버로딩 되어있을 경우의 예시이다.

// MyDervied가 MyBase 를 상속
public class MyDerived : MyBase
{
	// 생략
}

// MyImplement가 IMyInterface를 구현
public class MyImplement : IMyInterface
{
	// 생략
}

// 두가지 경우 모두 MyMethod<T>(T obj)가 호출됨
MyMethod(new MyDerived());
MyMethod(new MyImplement());

// 명시적 형변환을 해줘야 오버로딩된 메서드가 호출됨
MyMethod(new MyDerived() as MyBase);
MyMethod(new MyImplement() as IMyInterface);
```

위처럼 명시적 형변환을 하지 않는 경우에는 제네릭 클래스가 선택되기 때문에 잘못된 사용을 야기할 수 있다.

때문에 베이스 클래스나 인터페이스에 대해서 제네릭을 특화하는 것은 권장되지 않는다.

만약 제네릭을 특화하기로 결졍했다면 해당 타입뿐만 아니라 이 타입을 상속한 모든 파생 타입에 대해서도 특화를 수행해야 한다.
