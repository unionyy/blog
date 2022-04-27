---
title: "[Effective C#] Chapter 3: 제네릭"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-27T22:57:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-chapter3/
---
---
Effective C# Chapter 3 제네릭 개요
<!--more-->

### 제네릭
타입을 매개변수로 받는 것. (List<T>)

### 닫힌 제네릭 타입
구체적인 타입이 정해진 제네릭 (List<int>)

### 열린 제네릭 타입
일부의 타입 매개변수만 구체적인 타입이 정해진 제네릭 (List<T>)

### 머신 코드 공유 (참조 타입 vs 값 타입)
참조 타입을 사용하는 제네릭들은 같은 머신 코드를 공유한다.
```cs
List<string> stringList = new List<string>();
List<Stream> OpenFiles = new List<Stream>();
List<MyClassType> anotherList = new List<MyClassType>();
```
위의 코드들은 모두 같은 머신 코드를 공유한다.

값 타입을 사용하는 제네릭들은 서로 다른 머신 코드를 생성한다.
```cs
List<double> doubleList = new List<double>();
List<int> markers = new List<int>();
List<MyStruct> values = new List<MyStruct>();
```
위의 코드들은 각기 다른 머신 코드를 사용한다.

타입별로 다른 코드가 생성되면 런타임에 메모리 풋프린트가 커진다.

그러나 박싱 & 언박싱을 피할 수 있어서 결국 코드와 데이터 크기가 줄어든다.

또한 컴파일러가 타입 안정성을 보장해주므로 코드의 길이가 짧아진다.

## Reference
* [제네릭 형식 매개 변수](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/generics/generic-type-parameters){:target="_blank"}
* [C# 제네릭 1](https://blog.thehago.site/28){:target="_blank"}
* [C# 컴파일 및 실행 과정](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=thunderyk&logNo=221283105707){:target="_blank"}
* [.NET 환경의 컴파일 과정](https://rito15.github.io/posts/cs-dotnet-compile/){:target="_blank"}