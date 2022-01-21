---
title: "[C#] Delegate, 익명 함수, 람다식, Closure"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-01-22T00:40:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /cs-delegate/
---
---

## Delegate
Delegate는 delegate 타입을 만들 때 사용되는 클래스이다.

delegate 타입은 특정 형식의 메서드를 참조할 수 있는 타입이다.

(인스턴스 메서드, 스태틱 메서드 모두 참조할 수 있다)

<!--more-->

"delegate" 타입을 선언하면 컴파일러가 자동으로 Delegate 클래스를 상속하는 클래스를 생성해 준다.

(Delegate 클래스는 delegate 타입이 아니며, delegate 타입의 베이스 클래스일 뿐이다)

Delegate 클래스를 상속한 클래스를 만들 수 있는 권한은 시스템과 컴파일러만 갖고 있다.

때문에 Delegate 클래스를 상속한 delegate 타입을 상속하는 새로운 타입을 만들 수 없다.

```cs
// Delegate 선언 (컴파일러가 Delegate 클래스를 상속한 Del 클래스를 생성해준다)
public delegate int Del(int num);
```


## 익명 함수 (C# 2.0 부터 지원)
익명 함수는 메서드를 생성하지 않고도 delegate를 생성할 수 있게 해준다.

미리 정의된 메서드를 delegate에 참조시키는 대신에 inline 형태로 메서드를 생성해서 delegate가 참조할 수 있도록 해준다.

익명함수를 생성할 때에는 delegate라는 표현을 사용한다.

```cs
// anonymous method (C# 2.0)
Del del3 = delegate(int a) { return a + 1;};
```


## 람다식 (C# 3.0 부터 지원)
람다식은 익명 함수를 쉽게 표현하는 식이다.

람다식은 delegate 타입 또는 expression tree 형태로 변환될 수 있다.

```cs
// lambda (C# 3.0)
Del del4 = (a) => a + 1;
```

{% include ad-contents.html %}

## Closure
익명 함수로 delegate를 생성할 때에 매개변수나 익명 함수 내에 선언된 변수가 아닌 외부 변수를 참조할 수 있는데, 이를 Closure라고 부른다.

```cs
class Program
{
    public void ClosureTest()
    {
		string msg = "one";
        Action action = delegate () { Console.WriteLine(msg); };
        msg = "two";
        action();
    }
}
```
위의 코드를 실행할 경우 "two"가 출력된다.

익명 함수는 값이 아니라 변수를 참조하기 때문이다.

### Closure의 동작 원리
컴파일러는 익명 함수를 메서드로 변환해준다.

이때 로컬 스코프를 벗어났을 때에 메서드가 사라지는 것을 방지하기 위해 중첩 형식이 선언된다. (클래스 안에 가상의 클래스가 선언됨)

이는 익명 함수의 형태에 따라 알맞는 형태로 선언된다.

* 외부 참조가 없을 때

```cs
	class Program
    {
        public void ClosureTest()
        {
            Action action = delegate () { Console.WriteLine("zero"); };
			      action();
        }

    	static void Main(string[] args)
        {
            Program program = new();
            program.ClosureTest();
        }
    }
```
![no-reference](/assets/post-images/cs-delegate/no-reference);
중첩 클래스가 선언되고 일부 메서드와 필드가 static으로 선언된다.

* 로컬 변수를 참조했을 때

```cs
    class Program
    {
        public void ClosureTest()
        {
            string msg = "one";
            Action action = delegate () { Console.WriteLine(msg); };
            action();
        }

    	static void Main(string[] args)
        {
            Program program = new();
            program.ClosureTest();
        }
    }
```
![local-variable](/assets/post-images/cs-delegate/local-variable);
중첩 클래스가 선언되고 msg가 인스턴스 변수로 추가된다.

* 멤버 변수를 참조했을 때

```cs
    class Program
    {
        string msg3 = "three";
        public void ClosureTest()
        {
            Action action = delegate () { Console.WriteLine(msg3); };
            action();
        }

    	static void Main(string[] args)
        {
            Program program = new();
            program.ClosureTest();
        }
    }
```
![member-variable](/assets/post-images/cs-delegate/member-variable);
중첩 클래스가 선언되지 않고 익명 함수가 인스턴스 메서드로 선언된다.

## Reference
* [Delegate Class](https://docs.microsoft.com/en-us/dotnet/api/system.delegate?view=netcore-1.0){:target="_blank"}
* [Delegate and lambdas](https://docs.microsoft.com/en-us/dotnet/standard/delegates-lambdas){:target="_blank"}
* [Lambda expression](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions){:target="_blank"}
* [기본 제공 참조 형식](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/reference-types){:target="_blank"}
* [Delegates with Named vs Anonymous Methods](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/delegates-with-named-vs-anonymous-methods){:target="_blank"}
* [C# Closure 이해하기](https://www.csharpstudy.com/DevNote/Article/26){:target="_blank"}
* [Closure in C#](https://blog.daum.net/creazier/15309707){:target="_blank"}
* [중첩 형식](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/nested-types){:target="_blank"}
* [ildasm.exe(IL 디스어셈블러)](https://docs.microsoft.com/ko-kr/dotnet/framework/tools/ildasm-exe-il-disassembler){:target="_blank"}
