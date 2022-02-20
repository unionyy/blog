---
title: "[Effective C#] 아이템 9: 박싱과 언박싱을 최소화하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-02-20T19:28:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item9/
---
---
박싱: 값 타입 → 참조 타입

언박싱: 참조 타입 → 값 타입
<!--more-->

박싱과 언박싱이 자주 일어나게 되면 성능에 악영향을 미친다.

* 제네릭 클래스와 제네릭 메서드를 활용하자.
* String.Format() 대신 보간문자열을 이용하자 (아이템 4 참조)
* .NET 1.x 의 컬렉션 대신 제네릭 컬렉션을 사용하자
