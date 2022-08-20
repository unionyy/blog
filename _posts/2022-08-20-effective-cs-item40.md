---
title: "[Effective C#] 아이템 40: 지연 수행과 즉시 수행을 구분하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-08-20T12:28:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item40/
---
---

## 선언적 코드 (Declarative code) VS 명령적 코드 (Imperative code)
선언적 코드는 무슨 작업을 해야하는 지 정의하는 코드이고,

명령적 코드는 어떻게 작업을 수행해야 하는지를 단계별로 세분화 해서 기술하는 코드 이다.

<!--more-->

## 명령적 코드
다음은 명령적 코드의 예시이다.

```cs
var answer = DoStuff(Method1(), Method2(), Method3());
```

위 코드는 다음 순서로 수행된다.

Method1() → Method2() → Method3() → DoStuff()

이처럼 매개변수를 먼저 개산하고 메서드를 호출하는 방식은 즉시 수행의 예이다.

## 선언적 코드
반면 람다나 쿼리식은 지연 수행이 되는 선언적 코드이다.

var answer DoStuff(() => Method1(), () => Method2(), () => Method3());

위 코드는 다음 순서로 수행된다.

DoStuff() → 필요할 경우 순서와 상관 없이 Method1(), Method2(), Method3()이 호출된다.


## 즉시 수행과 지연 수행
item37에서 살펴본 바와 같이, 즉시 수행과 지연 수행은 다르게 동작하며 결과가 다를 수 있다.

즉시 수행과 지연 수행이 같은 결과를 얻기 위해서는 매개변수와 메서드가 변경 불가해야 한다.

즉시 수행과 지연 수행이 같은 결과를 반환한다고 했을 때에 어떤 것을 선택해야 할까?

```cs
Math.PI
```

Math.PI가 호출할 때마다 파이 값을 계산한다고 가장하면, 파이 값을 매번 계산하는 것 보다 저장해두고 쓰는 것이 더 효율적일 것이다.

```cs
CalculatePrimeFactors(int)
```

매개변수로 전달된 값의 약수를 구하는 CalculatePrimeFactors(int)함수가 있다면, 모든 정수의 약수를 저장해두기 보다는 매번 계산하는 편이 효율적일 것이다.

위 두가지 경우 처럼 공간 비용과 계산 비용을 고려하여 무엇을 사용할 지 선택해야 한다.

혹은 두가지 방법을 혼합해서 사용하는 방법도 있다.

```cs
var cache = Method1();
var answer = DoStuff(() => cache, () => Method2(), () => Method3());
```

캐시를 사용하여 메서드의 효율을 높혔다.

추가로, item38에서 살펴 보았듯이 지연 수행은 원격 DB에서 수행될 수도 있다. (LINQ to SQL)
