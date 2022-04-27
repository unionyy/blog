---
title: "[Effective C#] 아이템 16: 생성자 내에서는 절대로 가상 함수를 호출하지 말라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-21T22:36:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item16/
---
---
생성자가 수행을 완료하기 전까지는 객체가 완전히 생성된 것이 아니다.

객체가 완전히 생성되지 않았을 때에 가상 함수를 호출하면 이상 동작을 일으킨다.
<!--more-->

```cs
class B
{
    protected B()
    {
        VFunc();
    }

    protected virtual void VFunc()
    {
        Console.WriteLine("VFunc in B");
    }
}

class Derived : B
{
    private readonly string msg = "Set by initializer";

    public Derived(string msg)
    {
        this.msg = msg;
    }

    protected override void VFunc()
    {
        Console.WriteLine(msg);
    }
}

// new Derived("Constructed in main");
// 실행 결과: "Set by initializer"
```
Derived의 생성자를 호출하면 다음과 같은 과정을 따른다.

맴버 변수 초기화 → 베이스 생성자(B()) 수행 → Derived 생성자 수행

생성자를 호출하면 맴버 변수가 초기화된 이후에 베이스 생성자가 수행된다. (베이스 클래스의 생성자를 명시적으로 호출하지 않을 경우 베이스 클래스의 디폴트 생성자가 암시적으로 호출된다)

VFunc()는 Derived 클래스에서 재정의한 함수로 호출된다. (C#은 런타임에 타입을 고려하여 함수를 호출하기 때문)

이 때문에 Derived의 생성자에서 msg가 this.msg = msg로 초기화되지 않은 상태에서 Derived.VFunc()가 수행된다.

이는 추상 함수에서도 마찬가지로 적용된다.
```cs
abstract class B2
{
    protected B2()
    {
        VFunc();
    }

    protected abstract void VFunc();
}

class Derived2 : B2
{
    private readonly string msg = "Set by initializer";

    public Derived2(string msg)
    {
        this.msg = msg;
    }

    protected override void VFunc()
    {
        Console.WriteLine(msg);
    }
}

// new Derived("Constructed in main");
// 실행 결과: "Set by initializer"
```

C#은 파생 클래스에서 추상 함수를 반드시 구현해야 정상적으로 컴파일된다. 때문에 생성자 내에서 추상 함수를 호출했을 때에 런타임 예외를 피할 수 있다.

그러나 생성자 내에서 추상 함수를 호출하게 되면 가상 함수와 마찬가지로 의도치 않은 오류가 발생할 수 있다. 위 코드에서 msg는 변경이 불가능하도록 readonly로 선언되었으나 의도하지 않게 다른 값으로 변경된다.

베이스 클래스의 생성자 내에서 가상 함수를 호출하면 파생 클래스가 가상 함수를 어떻게 구현 했는 지에 따라 매우 민감하게 동작한다. 이는 매우 취약한 코드가 돼버린다.

따라서 생성자 내에서는 가상함수를 절대로 호출하지 않는 것이 좋다.

## Reference
* [Inheritance](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/){:target="_blank"}