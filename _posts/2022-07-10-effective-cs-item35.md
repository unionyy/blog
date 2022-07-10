---
title: "[Effective C#] 아이템 35: 확장 메서드는 절대 오버로드하지 마라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-10T15:56:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item35/
---
---

챕터 3의 아이템 27, 28에서 확장 메서드를 작성해야 하는 이유를 살펴보았다.

아이템 35는 확장 메서드를 사용하지 말아야 하는 경우이다.

<!--more-->

### 확장 메서드를 사용하지 말아야 하는 경우
* 타입을 확장하려 하는 경우
* 네임스페이스를 달리하여 여러 버전의 확장 메서드를 정의하는 경우


### 간단히 정의한 Person 클래스
```cs
public sealed class Person
{
    public string FirstName
    {
        get; set;
    }
    public string LastName
    {
        get; set;
    }
}
```

### Person 클래스의 확장 메서드 (콘솔 출력용 string을 반환하는 Format() 메서드)
```cs
// 좋지 않은 시작
// 확장 메서드를 이용하여 클래스를 확장함
namespace ConsoleExtensions
{
    public static class ConsoleReport
    {
        public static string Format(this Person target) =>
            target.FirstName + "." + target.LastName;
    }
}
```


### 확장 메서드 사용
```cs
using ConsoleExtensions;

Person person = new Person{
    FirstName = "Uniony",
    LastName = "me"
};

Console.WriteLine(person.Format());
```


### 확장 메서드 추가 (XML 파일 출력용 Format() 메서드)
```cs
namespace XmlExtenstion
{
    public static class XmlReport
    {
        public static string Format(this Person target) {
            // 생략
        }
    }
}
```


네임스페이스를 달리하여 두가지 Format() 확장 메서드를 정의하였다.

이 경우 using문에 따라 코드의 동작이 바뀌게 되므로 위험하다.


위의 예시의 경우 타입의 기능을 확장한다기 보다는 타입을 이용하는 메서드에 가깝다.

따라서 Format() 메서드가 Person 타입 내에 있을 필요가 없다.

```cs
public static class PersonReports
{
    public static string FormatAsText(Person target) =>
        target.FirstName + "." + target.LastName;

    public static string FormatAsXml(Person target)
    {
        // 생략
    }
}
```
위 코드와 같이 두가지 Format() 메서드를 동일한 클래스 내에서 다른 이름을 가진 메서드로 작성하는 것이 좋다.
