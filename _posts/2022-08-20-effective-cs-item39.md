---
title: "[Effective C#] 아이템 39: function과 action 내에서는 예외가 발생하지 않도록 하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-08-20T12:25:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item39/
---
---

일련의 값을 순차적으로 처리하는 코드가 중간에서 예외를 일으키면 상태를 복구할 수 없는 문제에 봉착한다.

<!--more-->

다음은 전체 직원들의 월급을 5% 인상해주는 코드이다.

```cs
var allEmployees = FindAllEmployees();
allEmployees.ForEach(e => e.MonthlySalary *= 1.05M);
```

이 루틴에서 예외가 발생하면 일부 직원의 월급만 인상된 상태에서 모든 직원의 월급을 원복하기 어렵다.

이를 해결하는 가장 쉬운 접근방법은 람다 표현식으로 나타낸 액션 메서드가 절대 예외를 발생시키지 않도록 하는 것이다.

```cs
allEmployees.FindAll(
    e => e.Classification == EmployeeType.Active).
    ForEach(e => e.MonthlySalary *= 1.05M);
```

이미 퇴사한 직원에 대한 정보가 남아 있어 예외가 발생한다고 가정하면,
위 코드와 같이 필터를 추가하여 예외를 피할 수 있다.

하지만 때로는 예외가 발생하지 않도록 코드를 작성하는 것이 불가능한 경우도 있다.

이런 경우, 예외 발생 가능성을 염두에 두고 코드를 작성할 수 있다.

```cs
var updates = (from e in allEmployees
	select new Employee
	{
		EmployeeID = e.EmployeeID,
		Classification = e.Classification,
		YearsOfService = e.YearsOfService,
		MonthlySalary = e.MonthlySalary *= 1.05M
	}).ToList();
allEmployees = updates;
```

위와 같이 원본 시퀀스의 복사본을 마련해두고 모든 작업이 완료된 경우에만 시퀀스를 대체하는 방식이 있다.

이 경우 시퀀스가 수정되는 도중에 예외가 발생할 경우 원본 시퀀스가 수정되지 않는다.

다만 이렇게 알고리즘을 변경하면 추가 비용이 많이 든다.

코드의 양도 늘었고, 원본 시퀀스 전체를 복사해야 하므로 성능 비용도 훨씬 커진다.
