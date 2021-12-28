---
title: "[Effective C#] 아이템 1: 지역변수를 선언할 때는 var를 사용하는 것이 낫다"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-12-29T08:35:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item1/
---
---
타입이 명시적으로 드러나야 하는 경우나 내장 숫자 타입 (int, float, double)을 선언하는 경우를 제외하고는 var를 사용하는 것이 좋다.
<!--more-->

## 중복 코드 제거
```cs
List<int> xs = new List<int>(); // explicit
 
var xs = new List<int>();     // implicit
 
List<int> xs = new();             // new(C# 9.0)
```
중복 코드를 최소화함으로써 실수를 줄이고 가독성을 향상시킬 수 있다.


## Anonymous types(익명 타입, 사용할 수 밖에 없음)
```cs
var uniony = new { team = "AppDev", age = "26" };
```
익명 타입의 오브젝트를 생성하기 위해서는 var를 사용해야한다.
익명 타입을 사용함으로써 한번만 사용할 객체를 명시적으로 정의할 필요 없이 생성할 수 있다.

참고: [Anonymous Types](https://docs.microsoft.com/ko-kr/dotnet/csharp/fundamentals/types/anonymous-types){:target="_blank"}

## 다형성 문제
C# 부모 클래스로의 암시적 형변환이 허용되는 등의 다형성을 지원한다.

때문에 변수를 명시적으로 선언할 경우에는 의도치 않게 암시적 형변환이 일어날 수 있다.

var를 사용하면 이를 방지할 수 있다.

```cs
// IQueryable<T> 가 IEnumerable<T>로 형변환 됨.
IEnumerable<string> q =
    from k in db.Krews
    select k.Name;
 
// 컴파일러가 q를 IQueryable<T> 로 추론함
var q =
    from k in db.Krews
    select k.Name;
```

LINQ 쿼리는 IEnumerable<T> 를 반환하기도 하고, IQueryable<T> 를 반환하기도 한다.

IQueryable<T> 가 IEnumerable<T> 를 상속하므로 명시적 선언을 사용했을 경우, 의도치 않은 형변환이 일어날 수 있다.

## Reference
* var - https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/var
* 암시적 형식 지역 변수 - https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/implicitly-typed-local-variables
* LINQ 쿼리 작업의 형식 관계 - https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations
* Anonymous Types - https://docs.microsoft.com/ko-kr/dotnet/csharp/fundamentals/types/anonymous-types
