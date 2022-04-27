---
title: "[Effective C#] 아이템 14: 초기화 코드가 중복되는 것을 최소화하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-18T20:49:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item14/
---
---
생성자를 작성할 때에 동일한 코드를 반복적으로 사용해야 한다면 공용 생성자나 기본값을 갖는 매개변수를 취하는 생성자를 작성하면 좋다.
<!--more-->

## 공용 생성자
```cs
public class Item14
{
	// 데이터 컬렉션
	private List<ImportantData> col1;
	
	// 인스턴스 변수
	private string name;

	public Item14() :
		this(0, "")
	{
	}

	public Item14(int initialCount) :
		this(initialCount, string.Empty)
	{
	}

    public Item14(string name) :
        this(0, name)
    {
    }

	public Item14(int initialCount, string name)
	{
		col1 = (initialCount > 0) ?
			new List<ImportantData>(initialCount) :
			new List<ImportantData>();

		this.name = name;
	}
}
```
생성자 4개를 만들었지만 하나의 생성자를 공용 생성자로 사용했다.

{% include ad-contents.html %}

## 기본값을 갖는 매개변수를 취하는 생성자
```cs
public class Item14_2
{
	// 데이터 컬렉션
	private List<ImportantData> col1;
	
	// 인스턴스 변수
	public string name;

	// new() 제약 조건을 만족시키려면 이 생성자가 필요하다.
	public Item14_2() :
		this(0, string.Empty)
	{
	}

	public Item14_2(int initialCount = 0, string name = "")
	{
		col1 = (initialCount > 0) ?
			new List<ImportantData>(initialCount) :
			new List<ImportantData>();

		this.name = name;
	}
}
```
여러가지 조합의 생성자를 만들 필요가 없어지므로 코드의 양을 최소화할 수 있다.

* 모든 매개변수에 대해서 기본값을 정의한 경우 new Item14_2()라고만 작성해도 유효하다.
* new() 제약 조건을 명시한 제네릭 클래스와 사용할 때에는 매개변수가 없는 생성자를 구현해야한다.
* 매개변수 기본값은 컴파일 타임 상수만을 지정할 수 있다.
* 타입을 사용하는 코드와의 결합도가 높아진다. (매개변수 이름, 기본값이 공개 인터페이스의 일부가 된다)


{% include ad-contents.html %}

## Private 메서드를 사용할 경우
```cs
public class Item14_3
{
	// 데이터 컬렉션
	private List<ImportantData> col1;
	
	// 인스턴스 변수
	private string name;

	public Item14_3()
	{
		commonConstructor(0, "");
	}

	public Item14_3(int initialCount)
	{
		commonConstructor(initialCount, "");
	}

    public Item14_3(string name)
    {
		commonConstructor(0, name);
    }

	public Item14_3(int initialCount, string name)
	{
		commonConstructor(initialCount, name);
	}

	private commonConstructor(int initialCount, string name)
	{
		col1 = (initialCount > 0) ?
			new List<ImportantData>(initialCount) :
			new List<ImportantData>();

		this.name = name;
	}
}
```

위 코드는 첫번째 코드와 비슷해보이지만 훨씬 비효율적인 코드를 생성한다.

### 호출 순서

모든 인스턴스 변수 초기화 → 베이스 클래스 생성자 호출 → commonConstructor 호출

이 과정에서 컴파일러는 중복 코드를 제거하지 못한다.

또한 유틸리티 함수는 readonly 상수를 초기화할 수 없다. (생성자 외부에서는 readonly 값을 변경할 수 없다)

## 첫번째 인스턴스를 생성할 때 수행되는 과정
1. 정적 변수의 저장 공간을 0으로 초기화
2. 정젹 변수에 대한 초기화 구문 수행
3. 베이스 클래스의 정적 생성자 수행
4. 정적 생성자 수행
5. 인스턴스 변수의 저장 공간을 0으로 초기화
6. 인스턴스 변수에 대한 초기화 구문 수행
7. 적절한 베이스 클래스의 인스턴스 생성자 수행
8. 인스턴스 생성자 수행
* 추가 인스턴스 생성시에는 5번 부터 수행한다.
* 컴파일러가 생성자 내에 중복된 맴버 초기화 코드를 생성하지 않도록 6, 7 단계는 최적화 되어 있다.


### 참고
* [new 제약 조건](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/new-constraint){:target="_blank"}
