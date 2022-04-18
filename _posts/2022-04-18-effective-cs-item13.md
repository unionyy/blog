---
title: "[Effective C#] 아이템 13: 정적 클래스 멤버를 올바르게 초기화하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-18T20:38:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item13/
---
---
인스턴스 멤버 초기화와 마찬가지로 정적 멤버를 간단히 초기화하는 경우에는 멤버 초기화 구문을 사용하는 것이 좋다.
<!--more-->

## 정적 멤버 변수를 초기화 하는 두 가지 방법
* 정적 멤버 초기화 구문
* 정적 생성자

static readonly로 선언된 변수는 위 두 가지 방법으로만 할당될 수 있다.

## 정적 생성자
```cs
// 정적 생성자
static MySingleton2()
{
	theOneAndOnly = new MySingleton2();
}
```
* 타입 내에 정의된 모든 메서드, 변수, 속성에 최초로 접근하기 전에 자동으로 호출되는 메서드.
* 초기화 과정이 복잡한 경우에 사용하면 좋다. (예외 처리 등)
* 하나만 가질 수 있다.
* 인자를 넘길 수 없다. (CLR을 통해서만 호출되기 때문)
* 두번 이상 호출되지 않는다. (예외가 발생해도 다시 호출되지 않는다)

### 참고

* [정적생성자](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/static-constructors){:target="_blank"}
