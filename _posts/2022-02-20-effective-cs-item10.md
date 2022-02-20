---
title: "[Effective C#] 아이템 10: 베이스 클래스가 업그레이드된 경우에만 new 한정자를 사용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-02-20T19:29:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item10/
---
---
new 한정자를 사용하면 베이스 클래스에서 정의한 메서드를 재정의할 수 있다.

그러나 virtual로 선언되지 않은 메서드를 재정의할 경우 매서드의 동작 방식이 모호해진다.
<!--more-->

```cs
// MyOtherClass가 MyClass를 상속
 
object c = MakeObject();
 
// MyClass 타입의 참조를 이용하여 메서드를 호출한다.
MyClass cl = c as MyClass;
cl.MagicMethod();
 
// MyOtherClass 타입의 참조를 이용하여 메서드를 호출한다.
MyOtherClass cl2 = c as MyOtherClass;
cl2.MagicMethod();
```
일반적인 경우 new 한정자는 사용하지 않는 것이 좋다.

다만 베이스 클래스가 업그레이드되어 동일한 메서드가 생긴 경우에는 new 한정자의 사용을 고려해볼만 하다.

* 베이스 클래스의 메서드가 기존의 메서드와 동일한 기능을 하는 경우: 베이스 클래스의 메서드를 사용하면 된다.
* 다른 기능을 하는 경우: 가능하면 기존의 메서드 이름을 변경하는 것이 좋다. (장기적으로 봤을 때 좋다)
* 해당 메서드가 어디에서 사용되고 있는지 정확히 파악하기 힘든 경우에만 new 한정자 사용을 고려하는 것이 좋다.
