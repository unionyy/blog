---
title: "[Effective C#] 아이템 18: 반드시 필요한 제약 조건만 설정하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-27T22:57:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item18/
---
---
C# 컴파일러는 제네릭 타입에 대해 올바른 IL을 생성해야 한다.
<!--more-->

이는 컴파일러에게 타입 매개변수에 대한 충분한 정보가 제공되지 않는 경우에도 동일하다.

따라서 타입 매개변수의 타입에 대한 추가 정보가 없으면 컴파일러는 이를 System.Object의 기능만을 제공하는 타입이라고 가정한다.

제약 조건은 컴파일러와 다른 개발자에게 타입 매개변수의 추가 정보를 알려주는 역할을 한다.

### 제약 조건을 사용했을 때의 장점
* 형변환이 가능한 지 체크하는 코드가 필요 없으므로 메서드를 간결하게 작성할 수 있다.
* 타입 매개변수로 잘못된 타입을 설정할 경우 컴파일 타임에서 오류를 확인할 수 있다.

## C# 제네릭의 특징
C# 컴파일러는 제약 조건에 설정된 정보만을 이용하여 IL을 생성한다.
```cs
public static bool AreEqual<T>(T left, T right) =>
	left.Equals(right);
```
위의 코드에서 IEquatable <T> 제약 조건이 설정되어 있다면 IEquatable<T>.Equals()가 호출된다.

그러나 제약 조건이 설정되어 있지 않다면 System.Object.Equals()가 호출된다. (타입이 IEquatable<T>를 구현하고 있을 때에도 동일)

위에서 정의한 AreEqual<T> 함수는 타입이 IEquatable<T>를 구현하고 있다면 IEquatable<T>.Equals()를 호출하고,

IEquatable<T>를 구현하고 있지 않다면 System.Object.Equals()를 호출하는 형태가 되는 것이 효율적이다.

이를 구현하기 위한 방법은 두 가지가 있다.

### 1. 오버로드 메서드를 제공한다.

### 2. 런타임에 특정 인터페이스를 구현 했는지 또는 특정 베이스 클래스를 상속 했는지를 확인한 후에 더 나은 메서드를 사용한다. (Equatable<T>, Comparable<T>)
new 대신 default()를 사용하면 new() 제약 조건이 필요 없을 수도 있다

default() 연산자는 특정 타입의 기본 값을 가져온다.

값 타입일 경우에는 0을, 참조 타입일 경우에는 null을 가져온다.

기본생성자 (new T())를 반드시 호출해야 하는 경우가 아니면 default()를 적절히 활용하여 new() 제약 조건을 사용하지 않는 것이 좋다.
