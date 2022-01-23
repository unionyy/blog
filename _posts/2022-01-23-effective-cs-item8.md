---
title: "[Effective C#] 아이템 8: 이벤트 호출 시에는 null 조건 연산자를 사용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-01-23T21:29:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item8/
---
---
이벤트 핸들러들을 결합시키면 이벤트를 한번에 호출할 수 있다.

그러나 결합되어 있는 이벤트 핸들러가 없는 경우에는 주의가 필요하다.
<!--more-->

```cs
public class EventSource
{
    private EventHandler<int> Updated;

    public void RaiseUpdates()
    {
        counter++;
        Updated(this, counter);
    }

    public void AddEvent(EventHandler<int> event1) {
        Updated += event1;
    }

    private int counter;
}
```
RaiseUpdates() 가 호출되어 Updated(this, counter); 이 호출되었을 때,

Updated에 결합된 이벤트 핸들러가 없다면 NullReferenceException이 발생한다.


```cs
public void RaiseUpdates()
{
     counter++;
     if(Updated != null)
          Updated(this, counter);
}
```
코드를 위와 같이 수정하면 NullReferenceException을 피할 수 있다.

그러나 매우 드물게 예외가 발생할 가능성이 있다.

if문으로 Updated를 체크한 뒤 Updated(this, counter);이 실행되기 직전에 다른 쓰레드에서 Updated의 이벤트를 해제하여 이벤트 핸들러가 비어있게 되는 경우이다.

이는 매우 드물게 발생하고, 재현하기 어려워서 분석 및 수정하기에 까다롭다.


```cs
public void RaiseUpdates()
{
     counter++;

     EventHandler<int> updated = Updated;
     if(updated != null)
          updated(this, counter);
}
```
위와 같이 이벤트핸들러를 지역변수로 선언하여 호출하면 문제를 해결할 수 있다.

다만 코드가 길어지고 가독성도 좋지 않다.


```cs
public void RaiseUpdates()
{
	counter++;

    Updated?.Invoke(this, counter);
}
```
위와 같이 null 조건 연산자( ? )를 사용하면 간결하게 이벤트 호출 코드를 작성할 수 있다.

## Reference
* [이벤트](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/events/){:target="_blank"}
