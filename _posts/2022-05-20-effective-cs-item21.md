---
title: "[Effective C#] 아이템 21: 타입 매개변수가 IDisposable을 구현한 경우를 대비하여 제네릭 클래스를 작성하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-05-20T23:54:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item21/
---
---

제네릭을 사용할 때에 타입이 IDisposable을 구현하고 있다면 리소스 누수가 발생할 수 있다.

<!--more-->

```cs
public interface IEngine
{
	void DoWork();
}

public class EngineDriverOne<T> wher T : IEngine, new()
{
	public void GetThingsDone()
	{
		T driver = new T();
		driver.DoWork();
	}
}
```
위 코드에서 driver가 IDisposable을 구현하고 있다면 리소스 누수가 발생할 수 있다.


## 해결방법 1. using 문 사용
```cs
public void GetThingsDone()
{
	T driver = new T();
	using (driver as IDisposable)
	{
		driver.DoWork
	}
}
```
driver가 IDisposable을 구현했을 경우 using 블럭이 종료될 때 Dispos() 메서드가 호출된다.

driver가 IDisposable을 구현하지 않은 경우에는 using(null)이 되어서 using 블럭이 종료될 때 Dispose()가 호출되지 않는다.


## 해결방법 2. IDisposable 구현
```cs
public sealed class EngineDriver2<T> : IDisposable where T : IEngine, new()
{
	// 생성 작업이 오래걸릴 수도 있으므로, Lazy를 이용하여 초기화
	private Lazy<T> driver = new Lazy<T>(() => new T());

	public void GetThingsDone() => driver.Value.DoWork();

	// IDisposable 멤버
	public void Dispose()
	{
		if (driver.IsValueCreated)
		{
			var resource = driver.Value as IDisposable;
			resource?.Dispose();
		}
	}
}
```

{% include ad-contents.html %}

매개변수로 전달한 타입(T)으로 멤버 변수를 선언한 경우에는 IDisposable을 직접 구현해야한다.

Dispose() 메서드를 구현하였고, 클래스에 sealed를 추가해 파생클래스를 만들 수 없도록 했다.

sealed를 추가하지 않을 경우 표준 Dispose 패턴 전체를 구현해야 한다. (아이템 17: 표준 Dispose 패턴을 구현하라)


## 해결방법 3. Dispose 호출 책임을 외부로 전담
```cs
public sealed class EngineDriver<T> where T : IEngine
{
	// null로 초기화
	private T driver;

	public EngineDriver(T driver)
	{
		this.driver = driver;
	}

	public void GetThingsDone()
	{
		driver.DoWork();
	}
}
```

매개변수로 전달한 타입으로 선언한 멤버 변수를 생성자의 매개변수로 받으면 IDispose를 구현하지 않아도 된다.

Dispose의 호출 책임을 제네릭 클래스 외부로 전담시키고, 객체의 소유권을 제네릭 클래스 외부로 옮기는 방식이다.

이때에는 new() 제약 조건도 제거할 수 있다.

## Reference
* [using 문](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/using-statement){:target="_blank"}
* [Lazy<T> 클래스](https://docs.microsoft.com/ko-kr/dotnet/api/system.lazy-1?view=net-6.0){:target="_blank"}