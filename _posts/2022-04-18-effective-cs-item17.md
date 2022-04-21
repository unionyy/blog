---
title: "[Effective C#] 아이템 17: 표준 Dispose 패턴을 구현하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-21T22:44:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item17/
---
---
IDisposable 인터페이스를 상속 받고 Dispose 패턴을 구현하면 비관리 리소스를 안정적으로 관리할 수 있다.
<!--more-->

```cs
using System;

public class DisposableTest : IDisposable
{

	private bool disposed = false;

	public DisposableTest()
	{
		Console.WriteLine("This is DisposableTest Object.");
	}

	public void Dispose()
    {
		// 관리, 비관리 리소스 해제
		Dispose(true);
		// finalizer가 호출되지 않도록 함
		GC.SuppressFinalize(this);
    }

	protected virtual void Dispose(bool isDisposing)
    {
		// 중복 실행 방지
		if (disposed)
			return;
		if (isDisposing)
        {
			// 관리 리소스 해제
			Console.WriteLine("Close Managed Resources");
        }
		// 비관리 리소스 해제
		Console.WriteLine("Close Unmanaged Resources");

		disposed = true;
    }
	~DisposableTest()
    {
		// finalizer에서 비관리 리소스 해제
		Dispose(false);
    }
}
```
## 표준 Dispose 패턴 구현하는 방법

### 최상위 베이스 클래스
* IDisposable 인터페이스 구현
* 비관리 리소스를 포함하는 경우, finalizer 추가 (Dispose를 호출하지 않을 경우 대비)
* 리소스 정리 작업을 수행하는 가상 메서드 정의 (Dispose와 finalizer에서 호출)

### 파생 클래스
* 리소스 정리 작업을 수행하는 가상 메서드 재정의 (고유 리소스 정리 작업이 필요할 경우)
* 비관리 리소스를 포함하는 경우, finalizer 추가
* 베이스 클래스의 가상함수 재호출

## finalizer가 필요한 이유

사용자가 Dispose()를 반드시 호출할 것이라는 보장이 없기 때문이다.

## Dispose()가 호출될 때, finalizer 호출을 막는 이유

finalizer가 호출되면 객체가 finalizer 큐에 들어가게 되고 필연적으로 1세대가 높아지게 된다. (메모리를 더 오래 점유하고 GC가 더 많은 일을 하게 된다)

이미 Dispose 메서드를 통해 리소스들을 해제한 경우에는 굳이 finalizer를 호출할 필요가 없다.

## Dispose()와 finalizer는 리소스 정리 작업만 수행해야 한다

Dispose()나 finalizer에서 리소스 정리가 아닌 다른 작업을 수행하게 되면 객체 생명 주기와 관련된 심각한 문제가 일어날 수 있다.

finalizer에서 사라져야할 객체가 다시 살아나게 되면 다시 finalizer를 호출하여 객체를 삭제할 수 없다.

객체가 살아난 것처럼 보여도 온전히 살아난 것은 아니다. (베이스 클래스의 필드는 시간이 지나면 정리된다)

## 객체가 이미 정리되었을 경우

플래그(위의 코드에서 disposed)를 설정하여 객체가 이미 정리되었는지 체크해야 한다.

이미 정리된 객체의 멤버 메서드에 접근하려 할 경우 ObjectDisposed 예외를 발생시킨다.

## Reference
* [IDisposble 인터페이스](https://docs.microsoft.com/ko-kr/dotnet/api/system.idisposable?view=net-6.0){:target="_blank"}