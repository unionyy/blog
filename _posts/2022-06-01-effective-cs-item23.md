---
title: "[Effective C#] 아이템 23: 타입 매개변수에 대해 메서드 제약 조건을 설정하려면 델리게이트를 활용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-06-01T15:40:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item23/
---
---

인터페이스를 선언하고 제약조건으로 설정하면 다양한 제약 조건을 설정할 수 있다.

<!--more-->

실제로 IEnumerable<T> 나 IComparable<T>와 같은 인터페이스를 제약 조건으로 설정하는 경우는 많다.

다만 특정 클래스에 대해 제약조건을 설정하기 위하여 전용 인터페이스를 구현하도록 하는 것은 사용자들에게 혼란을 가중시킬 수 있으며 비효율적일 수 있다.

```cs
// AddFunc 메서드를 사용하 위해IAdd<T> 인터페이스 선언
public interface IAdd<T>
{
	T Add(T A);
}

// Add<T> 메서드에 IAdd<T> 제약조건 설정
public static class Example
{
	public static T Add<T>(T left, T right) where T : IAdd<T>
    {
        return left.Add(right);
    }
}
```
위와 같이 Add<T> 메서드를 구현하면 사용자가 IAdd<T>를 구현해야 한다.

```cs
// Add<MyClass> 메서드를 사용하기 위해 MyClass 클래스에서 IAdd<MyClass> 인터페이스를 구현했다.
public class MyClass : IAdd<MyClass>
{
    int value;
    public MyClass(int value)
    {
        this.value = value;
    }
    public MyClass Add(MyClass A)
    {
        return new MyClass(this.value + A.value);
    }
}

// 사용
MyClass a = new MyClass(1);
MyClass b = new MyClass(2);
Example.Add<MyClass>(a, b);
```

위와 같은 방법 대신, 델리게이트를 활용하여 필요한 메서드를 매개변수로 받는 방법이 있다.

```cs
public static class Example
{
	public static T Add<T>(T left, T right, Func<T, T, T> AddFunc) =>
		AddFunc(left, right);
}
```
AddFunc라는 메서드를 제약조건으로 설정하기 위해 인터페이스를 구현하는 대신 델리게이트를 활용하여 AddFunc 메서드를 매개변수로 받아서 활용했다.

이 경우 MyClass가 IAdd<MyClass> 인터페이스를 구현할 필요가 없다.

```cs
// MyClass2 클래스 간단히 구현 (IAdd<MyClass2> 인터페이스를 구현하지 않음)
public class MyClass2
{
    public int value;

    public MyClass2(int value)
    {
        this.value = value;
    }
}

MyClass2 a = new MyClass2(1);
MyClass2 b = new MyClass2(2);
Example.Add<MyClass2>(a, b, (left, right) => new MyClass2(left.value + right.value));
```

원하는 형태로 델리게이트를 정의하여 사용하면 특정 메서드를 제약조건으로 사용하고 싶은 대부분의 경우를 대체할 수 있다.

특정 메서드 뿐만 아니라 여러 메서드에서 동일한 델리게이트를 필요로 하는 경우에는 델리게이트를 생성자의 매개변수로 설정하면 된다.

## Reference
* [제네릭 메서드](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/generics/generic-methods){:target="_blank"}
* [where(제네릭 형식 제약 조건)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/where-generic-type-constraint){:target="_blank"}