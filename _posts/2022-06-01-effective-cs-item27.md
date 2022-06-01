---
title: "[Effective C#] 아이템 27: 인터페이스는 간략히 정의하고 기능의 확장은 확장 메서드를 사용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-06-01T15:53:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item27/
---
---

인터페이스에서 정의하는 멤버들은 이를 구현하는 클래스에서 반드시 구현해야한다.

<!--more-->

반드시 구현해야하는 멤버의 수는 최소한으로 하기 위해 노력하고, 확장 메서드를 통해 다양한 기능을 제공하는 것이 좋다.

추가로 확장 메서드를 이용하면 인터페이스에서 정의된 메서드의 기본 구현체를 제공해줄 수도 있다.

### 예시
```cs
// IFoo 인터페이스
public interface IFoo
{
    int Marker { get; set; }
}

// IFoo 의 확장 메서드
public static class FooExtenstions
{
    public static void NextMarker(this IFoo thing) =>
        thing.Marker += 1;
}

// IFoo의 구현체 MyType 클래스
public class MyType : IFoo
{
    public int Marker { get; set; }
}

// MyType 클래스에는 NextMarker() 메서드가 정의되어 있지 않지만 호출할 수 있다.
MyType t = new MyType();
t.Marker = 1;
t.NextMarker();
```

## Reference
* [확장 메서드1](https://www.csharpstudy.com/CSharp/CSharp-extension-method.aspx){:target="_blank"}
* [사용자 지정 확장명 메서드 구현 및 호출 방법](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/how-to-implement-and-call-a-custom-extension-method){:target="_blank"}