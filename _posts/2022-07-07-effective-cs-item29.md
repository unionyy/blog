---
title: "[Effective C#] 아이템 29: 컬렉션을 반환하기보다 이터레이터를 반환하는 것이 낫다"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-07T22:10:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item29/
---
---

## LINQ 개요
* LINQ는 C# 3.0에 추가된 기능이다.
* LINQ는 지연된 쿼리를 지원한다.
* 다양한 저장소에 대해 쿼리를 수행할 수 있다.
* 쿼리 제공자를 자유롭게 구현할 수 있다.

<!--more-->

이터레이터 메서드는 호출자가 요청한 시퀀스를 생성하기 위해서 yield return 문을 사용하는 메서드를 말한다.

```cs
public static IEnumerable<char> GenerateAlphabet()
{
	var letter = 'a';
	while (letter <= 'z')
	{
		yield return letter;
		letter++;
	}
]
```
이터레이터는 데이터를 필요할 때 생성한다.


다음은 first와 last를 매개변수를 입력 받아 이터레이터를 반환한다.
```cs
public static IEnumerable<char> GenerateAlphaSubset(char first, char last)
{
	if (first < 'a' || first > 'z')
		throw new ArgumentException("a <= first <= z 이어야 합니다.", nameof(first));
	if (last < first || last > 'z')
		throw new ArguemntException("first <= last <= z 이어야 합니다.", nameof(last));

	var letter = first;

	while (letter <= last)
	{
		yield return letter;
		letter++;
	}
}
```
위 코드의 문제점은 시퀀스의 첫 번째 요소가 요청될 때까지 매개변수 값이 유효한지를 확인하는 코드가 수행되지 않는다는 것이다.

이 경우에는 잘못된 인자를 넘겼을 때, 문제를 확인하고 해결하기 까다롭다.


이럴 때는 다음과 같이 수정해야 한다.
```cs
public static IEnumerable<char> GenerateAlphaSubset(char first, char last)
{
	if (first < 'a' || first > 'z')
		throw new ArgumentException("a <= first <= z 이어야 합니다.", nameof(first));
	if (last < first || last > 'z')
		throw new ArguemntException("first <= last <= z 이어야 합니다.", nameof(last));

	return GenerateAlphabetSubsetImpl(first, last);
}

private static IEnumerable<char> GenerateAlphaSubsetImpl(char first, char last)
{
 	var letter = first;

	while (letter <= last)
	{
		yield return letter;
		letter++;
	}
}
```
위와 같이 생성자와 시퀀스 생성 부분을 분리하면 이터레이터가 생성되는 시점에서 인자의 유효성을 검사하게 된다.


이터레이터 메서드를 이용하여 시퀀스를 생성하면 사용자들이 메서드를 폭넓게 사용할 수 있게 된다.

시퀀스를 이용하면 값을 하나씩 가져오기도 용이하고,

ToList() 나 ToArray()와 같은 확장 메서드를 이용하여 컬렉션을 생성하기도 용이하다.
