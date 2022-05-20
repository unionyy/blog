---
title: "[Effective C#] 아이템 22: 공변성과 반공변성을 지원하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-05-21T00:04:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item22/
---
---

## 공변성과 반공변성

* 공변성: X → Y 가 가능할 때, C<X> → C<Y> 가 가능

* 반공변성: X → Y 가 가능할 때, C<Y> → C<X> 가 가능

* 불변성: X → Y 가 가능하지만, C<X> → C<Y> 와 C<Y> → C<X> 모두 불가능

<!--more-->

## 공변성 예시

### IEnumerable<T>
MyDerived가 MyBase를 상속했을 때, (MyDerived → MyBase 가 가능) IEnumerable<MyDerived> → IEnumerable<MyBase> 이 가능하다.

```cs
// MyBase 클래스와 GetEnumerable 메서드를 간단히 정의
public class MyBase
{
    protected int num;

    public MyBase(int num)
    {
        this.num = num;
    }

    public static IEnumerable<MyBase> GetEnumerable()
    {
        for (var i = 0; i < 10; i++)
            yield return new MyBase(i);
    }
}

// MyDerived는 MyBase를 상속받고 GetMyDerviedEnumerable 메서드를 정의
public class MyDerived : MyBase
{
    public MyDerived(int num)
    : base(num)
    {
    }
    public static IEnumerable<MyDerived> GetMyDerivedEnumerable()
    {
        for (var i = 0; i < 10; i++)
            yield return new MyDerived(i);
    }
}

// 간단한 사용 예시
IEnumerable<MyBase> b = MyDerived.GetMyDerivedEnumerable();	// 가능
IEnumerable<MyDerived> a = MyBase.GetEnumerable();			// 컴파일 에러
```

## 반공변성 예시

### IComparable<T>
MyDerived가 MyBase를 상속했을 때, (MyDerived → MyBase 가 가능) IComparable<MyBase> → IComparable<MyDerived>가 가능하다.

```cs
// MyBase가 IComparable<MyBase>를 구현
public class MyBase : IComparable<MyBase>
{
    protected int num;

    public MyBase(int num)
    {
        this.num = num;
    }

    public int CompareTo(MyBase other) =>
        num.CompareTo(other.num);
}

// MyDerived가 MyBase를 상속, IComparable<MyDerived>를 구현
public class MyDerived : MyBase, IComparable<MyDerived>
{
    public MyDerived(int num)
    : base(num)
    {
    }

    public int CompareTo(MyDerived other) =>
        num.CompareTo(other.num);
}

// 사용 예시
IComparable<MyDerived> a = new MyBase(1) as IComparable<MyBase>;	// 가능
IComparable<MyBase> b = new MyDerived(1) as IComparable<MyDerived>;	// 컴파일 에러
```

{% include ad-contents.html %}

## 불변성 예시

### IList<T>
MyDerived가 MyBase를 상속했을 때, (MyDerived → MyBase 가 가능) IList<MyDerived> → IList<MyBase> 와 IList<MyBase> → IList<MyDerived> 모두 불가능하다.

```cs
// MyBase 간단히 구현
public class MyBase
{
    protected int num;

    public MyBase(int num)
    {
        this.num = num;
    }
}

// MyBase를 상속한 MyDervied를 간단히 구현
public class MyDerived : MyBase, IComparable<MyDerived>
{
    public MyDerived(int num)
    : base(num)
    {
    }
}

// 사용 예시
IList<MyBase> a = new List<MyDerived>();	// 컴파일 에러
IList<MyDerived> a = new List<MyBase>();	// 컴파일 에러
```

## in / out 데코레이터

제네릭 선언시에 in / out 데코레이터를 사용하면 공변과 반공변을 정의할 수 있다.

공변은 <out T>로 정의하고, 반공변은 <in T>로 정의한다.

in과 out을 사용하지 않고 <T>로만 정의되었을 경우에는 불변이다.

```cs
// IEnumerable<T>는 공변이므로 <out T>로 정의되어 있다.
public interface IEnumerable<out T> : System.Collections.IEnumerable

// IComparable<T>는 반공변이므로 <in T>로 정의되어 있다.
public interface IComparable<in T>

// IList<T>는 불변이므로 <T>로 정의되어 있다.
public interface IList<T> : System.Collections.Generic.ICollection<T>, System.Collections.Generic.IEnumerable<T>
```

## 델리게이트

델리게이트의 매개변수를 정의할 때에도 공변 / 반공변을 모두 사용할 수 있다.

메서드의 매개변수 타입은 반공변(in)이고 메서드의 반환 타입은 공변(out)이다.

Func<T> 의 T는 반환 타입이므로 공변이고, Action<T>의 T는 매개변수 타입이므로 반공변이다.

마찬가지로 Func<T, TResult> 의 T는 반공변, TResult는 공변이다.

```cs
// 공변
public interface ICovariantDelegates<out T>
{
	T GetAnItem();
	Func<T> GetAnItemLater();
	void GiveAnItemLater(Action<T> whatToDo);
}

// 반공변
public interface IContravariantDelegates<in T>
{
	void ActOnAnItem(T item);
	void GetAnItemLater(Func<T> item);
	Action<T> ActOnAnItemLater();
}
```

## Reference
* [[C#] 공변성과 반공변성이란?](https://sticky32.tistory.com/entry/C-%EA%B3%B5%EB%B3%80%EC%84%B1%EA%B3%BC-%EB%B0%98%EA%B3%B5%EB%B3%80%EC%84%B1%EC%9D%B4%EB%9E%80){:target="_blank"}
* [Action<T> 대리자](https://docs.microsoft.com/ko-kr/dotnet/api/system.action-1?view=net-6.0){:target="_blank"}
* [Func<T, TResult> 대리자](https://docs.microsoft.com/ko-kr/dotnet/api/system.func-2?view=net-6.0){:target="_blank"}
* [Func<TResult> 대리자](https://docs.microsoft.com/ko-kr/dotnet/api/system.func-1?view=net-6.0){:target="_blank"}