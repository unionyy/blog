---
title: "[Effective C#] 아이템 38: 메서드보다 람다 표현식이 낫다"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-22T17:40:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item38/
---
---

다음 코드는 동일한 람다 표현식이 반복되는 경우이다.

<!--more-->

```cs
var allEmployees = FindAllEmployees();

// 20년 이상 근속자
var earlyFolks = from e in allEmployees
	where e.Classification == EmployeeType.Salary
	where e.YearsOfService > 20
	where e.MonthlySalary < 4000
	select e;

//20년 미만 근속자
var newest = from e in allEmployees
	where e.Classification == EmployeeType.Salary
	where e.YearsOfService < 20
	where e.MonthlySalary < 4000
	select e;
```

위 코드의 반복되는 부분을 메서드로 정의하여 재사용 가능하도록 했다.
```cs
// 메서드로 분리
private static bool LowPaidSalaried(Employee e) =>
	e.MonthlySalary < 4000 && e.Classification ==
	EmployeeType.Salary;

var allEmployees = FindAllEmployees();

// 20년 이상 근속자
var earlyFolks = from e in allEmployees
	where LowPaidSalaried(e) &&
	e.YearsOfService > 20
	select e;

// 20년 미만 근속자
var newest = from e in allEmployees
	where LowPaidSalaried(e) && e.YearsOfService < 20
	select e;
```

그러나 위의 코드는 오히려 이전 코드보다 재사용성이 낮아진다.

쿼리 구문은 LINQ to Objects나 LINQ to SQL 의 과정을 수행하게 된다.

### LINQ to Objects
* IEnumerable<T>를 입력 시퀀스로 취한다.
* 로컬 저장소에 대한 쿼리 수행 방법이다. (컬렉션 등)
* 익명 델리게이트를 생성 & 활용한다.

### LINQ to SQL
* IQueryable<T>를 입력 시퀀스로 취한다.
* 표현식 트리를 사용한다.
* 트리로부터 T-SQL 쿼리를 생성한다.
* T-SQL 쿼리는 DB 엔진 내에서 수행된다.

쿼리 구문 내에 메서드를 호출하는 부분은 T-SQL 표현식으로 변환되지 못한다.
즉, LINQ to SQL 이 작동하지 않고 예외를 발생시킨다.

위처럼 메서드를 정의하는 방법 대신 닫힌 제네릭 타입에 대한 확장 메서드를 작성할 수 있다.
```cs
private static IQueryable<Employee> LowPaidSalariedFilter
	(this IQueryable<Employee> sequence) =>
		from s in sequence
		where s.Classification == EmployeeType.Salary &&
		s.MonthlySalary < 4000
		select s;

// 나머지 부문
var allEmployees = FindAllEmployees();

// 우선 필터링 수행
var salaried = allEmployees.LowPaidSalariedFilter();

// 20년 이상 근속자
var earlyFolks = salaried.Where(e => e.YearsOfService > 20);

// 20년 미만 근속자
var newest = salaried.Where(e => e.YearsOfService < 20);
```
위 코드는 LINQ to SQL 작업이 수행될 수 있다.

다만 LINQ to Objects까지 지원하기 위해서는 IEnumerable<Employee>를 매개변수로 취하는 오버로드 메서드를 구현해야 한다.

## Reference
* [IEnumerable<T> Return VS IQueryable<T> Return](https://rateye.tistory.com/1420){:target="_blank"}
* [Chap03-1. Transact-SQL 기본 - SELECT](https://excelsior-cjh.tistory.com/181){:target="_blank"}
