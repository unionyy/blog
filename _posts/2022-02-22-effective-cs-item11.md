---
title: "[Effective C#] 아이템 11: .NET 리소스 관리에 대한 이해"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-02-22T02:24:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item11/
---
---
.NET 개발에 있어서 메모리와 주요 리소스들이 어떻게 관리되는지 알아야 한다.
<!--more-->

가비지 컬렉터가 전반적인 메모리 관리를 해준다.

가비지 컬렉터는 응용프로그램의 최상위 객체로부터 개별 객체까지의 도달 가능 여부를 확인하도록 설계되어 있다.

개발자는 개별 객체 해제나 참조 관계로 인해 발생하는 복잡한 문제를 고민할 필요가 없다.

가비지 컬렉터가 수행되면 관리 힙의 콤팩트 작업을 수행한다.
(사용 중이거나 도달 가능한 객체들을 한곳으로 옮겨서 조각난 가용 메모리를 큰 공간으로 만든다)

![garbage collector](/assets/post-images/effective-cs-item11/garbage-collector.png)

## 비관리 리소스
OS 자원 등 비관리 리소스는 개발자가 직접 관리해야 한다.

.NET 프레임워크는 개발자가 비관리 리소스를 손쉽게 관리할 수 있도록 finalizer와 IDisposable 인터페이스를 제공한다.

finalizer를 사용할 경우 여러 단점들이 발생해서 IDisposable 인터페이스를 사용하는 것이 좋다.

## finalizer의 단점
finalizer가 어느 시점에서 호출될 지 알 수 없다.

finalizer를 가진 객체는 가비지 컬렉터가 수행될 때 바로 해제되지 않는다.
(가비지 컬렉터에는 세대 개념이 있기 때문에 해제되기 까지 더 오래 걸린다)


## Reference
* [.NET Framwork 기본 - 비관리 리소스 종결](https://blog.shovelman.dev/594){:target="_blank"}
