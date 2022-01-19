---
title: "[Effective C#] 아이템 7: 델리게이트를 이용하여 콜백을 표현하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-01-20T00:12:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item7/
---
---
델리게이트를 이용하면 타입안정적인 콜백을 정의할 수 있다.

인터페이스를 사용할 때보다 클래스간의 결합도를 낮출 수 있다.
<!--more-->

## Delegate 선언 및 사용
```cs
// Delegate 선언
public delegate int Del(int num);

// Delegate를 사용한 메서드
public static void Deltest(int a, Del del) {
    Console.WriteLine(del(a*2));
    return;
}
```

## Delegate 인스턴스 생성
```cs
static int myMethod(int a) {
  return a+1;
}
// 생성자
Del del1 = new Del1(myMethod);

// simpler (C# 2.0)
Del del2 = myMethod;

// anonymous method (C# 2.0)
Del del3 = delegate(int a) { return a + 1;};

// lambda (C# 3.0)
Del del4 = (a) => a + 1;
Deltest(1, del1);
// 3
```


## 멀티캐스트
Delegate는 기본적으로 멀티캐스트를 지원한다.

멀티캐스트 델리게이트를 호출하면, 대상 메서드들이 연속으로 호출된다.

다만 마지막 값만 반환되므로 주의해야한다.


* Delegate는 마지막 값만 반환한다.

```cs
Del myDel = (a) => a + 1;
myDel += (a) => a + 2;
myDel += (a) => a + 3;

Console.WriteLine(myDel.GetInvocationList().Length);
// 3

Deltest(1, myDel);
// 5 (Delegate가 마지막 메서드만 반환)
```

{% include ad-contents.html %}

* 모든 함수가 호출은 된다.

```cs
static int myPrintReturn(int a) {
	Console.WriteLine("Do myPrintReturn");
	return a + 1;
}

Del myDel2 = myPrintReturn;
myDel2 += myPrintReturn;
myDel2 += myPrintReturn;

Deltest(1, myDel2);
// Do myPrintReturn
// Do myPrintReturn
// Do myPrintReturn
// 3
```

* 여러개의 리턴값 사용하기

```cs
public static void DeltestMulti(int a, Del dels) {
     foreach(Del1 del in dels.GetInvocationList()) {
          Console.WriteLine(del(a*2));
     }
}

DeltestMulti(1, myDel);
// 3
// 4
// 5
```

foreach()와 GetInvocationList()를 사용하여 각각의 메서드를 실행하고 반환값을 가져올 수 있다.



## .NET Framework 라이브러리
Predicate<T>, Action<>, Func<> 와 같이 자주 사용되는 Delegate가 .NET Framework에 정의되어 있다.

## Reference
[C# delegate 기초](https://www.csharpstudy.com/CSharp/CSharp-delegate-concept.aspx){:target="_blank"}

[대리자 사용](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/delegates/using-delegates){:target="_blank"}

[.NET 성능 팁](https://docs.microsoft.com/ko-kr/dotnet/framework/performance/performance-tips){:target="_blank"}

[대리자를 선언, 인스턴스화, 사용하는 방법](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/delegates/how-to-declare-instantiate-and-use-a-delegate){:target="_blank"}

