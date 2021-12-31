---
title: "[Effective C#] 아이템 3: 캐스트보다는 is, as가 좋다"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-12-31T21:24:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item3/
---
---
아이템 3: 캐스트보다는 is, as가 좋다
<!--more-->

캐스트는 형변환 연산자가 개입될 수 있어서 일관성이 떨어진다.

```cs
public class SecondType
{
    private MyType _value;
 
    // 형변환 연산자 (SecondType -> MyType)
    public static implicit operator MyType(SecondType t)
    {
        return t.value;
    }
}
```
형변환 연산자는 객체의 런타임 타입이 아니라 컴파일타임 타입에 맞춰 수행된다.

```cs
object o = Factory.GetObject(); // o는 SecondType
 
// o는 SecondType
 
// as
MyType t = o as MyType // o 가 MyType이 아니면 null
 
// cast
MyType t1 = (MyType)o; // o가 MyType이 아니면 InvalidCastException
```
컴파일러는 o가 SecondType인지 모르기 때문에 둘 다 실패한다.

캐스트는 사용자 정의 형변환으로 인해 o가 선언된 타입에 따라 다른 결과를 반환한다.

as는 o가 선언된 타입에 상관 없이 동일한 결과를 반환한다. (일관성 높음)

더 방어적으로 코드를 작성하기 위해서는 is 연산자로 형변환이 가능한지 먼저 확인하면 된다.
```cs
o is MyType
```

## Reference
* [is 연산자](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/type-testing-and-cast#is-operator){:target="_blank"}
