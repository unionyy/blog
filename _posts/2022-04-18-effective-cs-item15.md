---
title: "[Effective C#] 아이템 15: 불필요한 객체를 만들지 말라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-18T20:56:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item15/
---
---
너무 많은 객체를 생성하고 제거하면 가비지 컬렉터가 많은 프로세스 시간을 사용하게 되고 이는 심각한 성능 문제를 일을킬 수도 있다.
<!--more-->

## 자주 사용되는 지역변수를 멤버 변수로 변경하라

모든 참조 타입 객체는 동적으로 메모리를 할당한다.

따라서 지역변수로 객체를 선언할 경우에, 해당 객체는 메서드를 벗어나는 순간 가비지가 된다.

자주 사용되는 참조 타입 객체는 멤버 변수로 변경하여 재사용되도록 하는 것이 좋다.


## 의존성 주입 (Dependency Injection)을 활용하여 자주 사용되는 객체를 재활용하라
```cs
private static Brush blackBrush;

public static Brush Black
{
	get
	{
		if (blackBrush == null)
			blackBrush = new SolidBrush(Color.Black);
		return blackBrush;
	}
}
```
검정색 브러시(blackBrush)는 여러 곳에서 사용되는 브러시이다.

검정색 브러시를 멤버 변수로 선언한다고 해도 객체를 생성할 때마다 검정색 브러시가 생성된다면 비효율적이다.

위 코드처럼 검정색 브러시를 정적 변수로 선언하면 여러 객체가 하나의 브러시를 재사용할 수 있다.

또한 검정색 브러시를 생성하지 않았다가 최초로 호출되었을 때에 생성하여 불필요한 객체 생성을 줄였다.

### 단점
* 생성된 객체가 메모리상에 필요 이상으로 오랫동안 남아 있을 수 있다.
* Dispose() 메서드의 호출 시점을 결정할 수 없기 때문에 비관리 리소스를 삭제할 수 없다.




## 변경 불가능한(immutable) 타입을 작성하는 경우 StringBuilder와 같은 기능을 함께 제공하는 것을 고려하라

string과 같이 변경 불가능한 타입은 수정이 불가능하다.
string을 수정할 경우 객체가 변경되는 것이 아니라 새로운 객체가 생성된다.
(이전 객체는 가비지가 된다)

따라서 문자열을 복잡하게 만들어야 하는 상황에서는 string이 아니라 StringBuilder 클래스를 이용하는 것이 좋다.
StringBuilder는 문자열을 생성, 수정, 변경할 수 있고 최종적으로 string 타입의 객체를 가져오는 기능을 제공한다.

string과 StringBuilder의 관계와 비슷하게,
변경 불가능한 타입을 작성하는 경우 이에 대응하는 변경 가능한 빌더 클래스를 같이 작성하는 것을 고려해보자.
