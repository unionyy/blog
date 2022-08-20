---
title: "[Effective C#] 아이템 42: IEnumerable<T> 데이터 소스와 IQueryable<T> 데이터 소스를 구분하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-08-20T12:38:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item42/
---
---

IEnumerable<T>와 IQueryable<T> 는 거의 동일한 API 정의를 가지고 거의 대부분 상호 교환이 가능하다.

하지만 항상 가능한 것은 아니며, 동작 방식과 성능에서 큰 차이가 난다.

<!--more-->

아이템 38에서 LINQ to SQL (IQueryable<T>)과 LINQ to Objects (IEnumerable<T>)의 차이점에 대해서 다뤘었다.

```cs
// 첫 번째 예
var q =
    from c in dbContext.Customers
    where c.City == "London"
    select c;

var finalAnswer = from c in q
                  orderby c.Name
                  select c;

// finalAnswer에 대한 순회 코드는 생략

// 두 번째 예
var q =
    (from c in dbContext.Customers
     where c.City == "London"
     select c).AsEnumerable();

var finalAnswer = from c in q
                  orderby c.Name
                  select c;

// finalAnswer에 대한 순회 코드는 생략
```

첫 번째 예는 일반적인 LINQ to SQL 쿼리이고 IQueryable<T>의 기능을 사용한다.

두 번째 예는 첫 번째 쿼리문이 IEnumerable<T> 시퀀스를 반환하므로 그 뒤의 쿼리는 LINQ to Objects가 수행된다.


아이템 37에서 살펴봤듯이 IQueryable<T>는 각각의 메서드를 분석하지 않는다.
```cs
private bool isValidProduct(Product p) =>
    p.ProductName.LastIndexOf('C') == 0;

// 다음 코드는 제대로 동작한다
var q1 =
	from p in dbContext.Products.AsEnumerable()
    where isValidProduct(p)
    select p;

// 다음 코드는 제대로 동작하지 않는다. (시퀀스를 순회활 때 예외 발생)
var q2 =
    from p in dbContext.Products
    where isValidProduct(p)
    select p;
```

q1의 쿼리는 AsEnumerable() 메서드로 데이터 컬렉션을 IEnumerable로 변환시켰다.

그래서 where 절의 isValidProduct(p) 메서드가 제대로 수행될 수 있다. (LINQ to Objects로 수행됨)

하지만 q2의 쿼리는 IQueryable<T>, 즉 LINQ to SQL로 수행되므로 isValidProduct(p) 메서드가 수행될 수 없다.


이렇게 IQueryable<T>가 IEnumerable<T>로 변환이 가능한 이유는 IQueryable<T>가 IEnumerable<T>를 구현하고 있기 때문이다.


반대로 AsQueryable() 메서드도 유용하게 쓰일 수 있는 경우가 있다.

```cs
public static IEnumerable<Product> ValidProducts(
	this IEnumerable<Product> products) =>
       from p in products.AsQueryable()
       where p.ProductName.LastIndexOf('C') == 0
       select p;
```

위 코드는 products가 IQueryable<Product>를 구현하고 있는 경우 LINQ to SQL이 수행되고,

그렇지 않은 경우에는 LINQ to Objects가 수행된다.

추가로 where 절에 쓰인 LastIndexOf('c')는 LINQ to SQL 라이브러리를 통해 올바르게 해석 가능한 메서드이다.
