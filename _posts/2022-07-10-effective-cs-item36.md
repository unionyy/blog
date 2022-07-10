---
title: "[Effective C#] 아이템 36: 쿼리 표현식과 메서드 호출 구문이 어떻게 대응되는지 이해하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-07-10T16:02:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item36/
---
---

C# 컴파일러는 쿼리 표현식을 메서드 호출 구문으로 변환해준다.

전체 쿼리 표현식 패턴에는 Where(), Select() 등 11개의 메서드가 포함되어 있다.

<!--more-->

System.Linq.Enumerable 클래스는 IEnumerable<T>에 대한 확장 메서드로 이 패턴을 구현하고 있다.

System.Linq.Queryable 클래스는 IQueryable<T> 에 대한 확장 메서드로 유사한 기능을 구현하고 있다.

따라서 IEnumerable<T>나 IQueryable<T>를 구현하면 이미 쿼리 표현식을 구현했다고 볼 수 있다.

### Where(), Select()
```cs
int[] numbers = { 0, 1, 2, 3, 4, 5 };

var smallNumbers = from n in numbers
    where n < 3
    select n;

// 위 쿼리문은 다음과 같이 변환된다.
var smallNumbers = numbers.Where(n => n < 5);


var allNumbers = from n in numbers select n;

// 위 쿼리문은 다음과 같이 변경된다.
var allNumbers = numbers.Select(n => n);
```

### OrderBy(), ThenBy(), OrderByDescending(), ThenByDescending()
```cs
var people = from e in employees
    where e.Age > 30
    orderby e.LastName, e.FirstName, e.Age descending
    select e;

// 위 쿼리는 다음과 같이 변환된다.
var people = employees.Where(e => e.Age > 30).
    OrderBy(e => e.LastName).
    ThenBy(e => e.FirstName).
    ThenByDescending(e => e.Age); 
```
OrderBy()는 요소를 특정 키에 대해서 오름차순으로 정렬한다.

ThenBy()는 정렬된 요소를 정렬 키가 동일한 요소들에 대해서 특정 키에 대해 오름차순으로 정렬한다.

OrderByDescending()과 ThenByDescending()은 말 그대로 내림차순 정렬이다.

### GroupBy()
쿼리식 안에 그룹핑이 포함되어 있거나 여러개의 from 절이 포함되면 여러 단계를 거쳐 메서드 호출 구문으로 변환된다.
```cs
var results = from e in employees
    group e by e.Department into d
    select new
    {
        Department = d.Key,
        Size = d.Count()
    };

// 먼저 group into 구문이 중첩 쿼리로 변환된다.
var results = from d in
    from e in employees group e by e.Department
    select new {
        Department = d.Key,
        Size = d.Count()
    };

// 이제 메서드 호출 구문으로 변환된다.
var results = employees.GroupBy(e => e.Department).
    Select(d => new { Department = d.Key, Size = d.Count() });

GroupBy() 메서드는 키, 값 리스트를 쌍으로 갖는 시퀀스를 생성한다.
```

키는 그룹을 선택하는 셀렉터 역할을 하고 값은 그룹 내의 항목을 나타낸다.

### SelectMany()
여러개의 from 절을 포함하는 쿼리식은 SelectMany() 메서드로 변환된다.
```cs
int[] odds = { 1, 3, 5, 7 };
int[] evens = { 2, 4, 6, 8 };

var values = from oddNumber in odds
    from evenNumber in evens
    where oddNumber > evenNumber
    select new
    {
        oddNumber,
        evenNumber,
        Sum = oddNumber + evenNumber
    };

// 위 쿼리는 다음과 같이 변환된다.
var values = odds.SelectMany(oddNumber => evens,
    (oddNumber, evenNumber) =>
    new { oddNumber, evenNumber }).
    Where(pair => pair.oddNumber > pair.evenNumber).
    Select(pair => new {
        pair.oddNumber,
        pair.evenNumber,
        Sum = pair.oddNumber + pair.evenNumber
    });

// 이해하기 쉽도록 SelectMany 메서드만 발췌했다.
odds.SelectMany(oddNumber => evens,
    (oddNumber, evenNumber) =>
    new { oddNumber, evenNumber });
```
SelectMany()의 첫 번째 매개변수는 첫 번째 입력 시퀀스에 두 번째 입력 시퀀스를 매핑하는 함수이다.

두 번째 매개변수는 두 입력 시퀀스를 이용하여 출력 시퀀스를 생성하는 함수이다.


만약 from 절이 n개(2개 이상) 사용되면, SelectMany() 메소드 n - 1 개가 연쇄적으로 생성된다.

### Join(), GroupJoin()
Join()과 GroupJoin() 모두 join 표현식에서 사용된다.

into 절이 포함된 경우 GroupJoin()이 사용되고, 포함되지 않은 경우 Join()이 사용된다.
```cs
var numbers = new int[] { 0, 1, 2, 3, 4, 5 };
var labels = new string[] { "0", "1", "2", "3", "4", "5" };

var query = from num in numbers
    join label in labels on num.ToString() equals label
    select new { num, label };

// 위 쿼리는 다음과 같이 변환된다.
var qeury = numbers.Join(labels, num => num.ToString(),
    label => label, (num, label) => new { num, label });

// into 절 사용
var groups = from p in projects
    join t in tasks on p equals t.Parent
    into projTasks
    select new { Project = p, projTasks };

// 위 쿼리는 다음과 같이 변환된다.
var groups = projects.GroupJoin(tasks,
    p => p, t => t.Parent,
    (p, projTasks ) => new { Project = p, TaskList = projTasks });
```

## Reference
* [쿼리 식 기본 사항(C#의 LINQ)](https://docs.microsoft.com/ko-kr/dotnet/csharp/linq/query-expression-basics){:target="_blank"}
* [내부 조인 수행(C#의 LINQ)](https://docs.microsoft.com/ko-kr/dotnet/csharp/linq/perform-inner-joins){:target="_blank"}
