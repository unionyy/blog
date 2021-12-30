---
title: "[Effective C#] 아이템 2: const 보다는 readonly가 좋다"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-12-30T09:24:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item2/
---
---
const(컴파일타임 상수) 보다는 readonly(런타임 상수)를 사용하는 것이 좋다.
<!--more-->

```cs
// 컴파일타임 상수
public const int Millennium = 2000;
 
// 런타임 상수
public static readonly int ThisYear = 2004
```

컴피알타임 상수가 런타임 상수보다 약간의 성능상의 이점이 있으나, 유연성이 떨어진다.

값이 절대로 바뀌지 않는 경우, 성능이 매우 중요한 경우, 컴파일 시 사용되는 상수값을 정의할 경우를 제외하고는 런타임 상수를 사용하는 것이 좋다.

## 모든 타입과 사용 가능
const는 내장 숫자형, enum, 문자열, null에 대해서만 사용 가능하다.

반면 readonly는 모든 타입과 함께 사용 가능하다.

생성자를 통해 초기화하는 것도 가능하다. (그 이후에는 수정될 수 없다)

```cs
// 컴파일타임 상수, 컴파일 X
private const DateTime joinKaKao = new DateTime(2021, 11, 17);
 
// 런타임 상수, 컴파일 O
private static readonly DateTime joinKaKao = new DateTime(2021, 11, 17);
```

## 참조 형식
const는 컴파일 시에 숫자나 문자를 직접 사용한 것과 동일한 IL코드를 생성한다.

그래서 public으로 선언된 컴파일상수를 변경할 경우, 해당 상수를 사용하는 모든 코드를 리빌드해야한다.

반면 readonly는 변수를 참조하는 형식으로 사용된다.

그래서 해당 어셈블리만 수정하면 전체 코드를 리빌드할 필요가 없다.



참조 형식으로 사용되므로 생기는 단점도 있다.

* 컴파일 상수보다 성능이 떨어질 수 있다.
* 특성 매개변수, switch/case 문의 레이블, enum 정의 등에서는 사용할 수 없다.

## Reference
* [Difference Between Const, ReadOnly and Static ReadOnly in C#](https://www.c-sharpcorner.com/UploadFile/c210df/difference-between-const-readonly-and-static-readonly-in-C-Sharp/){:target="_blank"}
