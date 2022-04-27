---
title: "[Effective C#] 아이템 20: IComparable<T>와 IComparer<T>를 이용하여 객체의 선후 관계를 정의하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-27T23:23:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item20/
---
---
## IComparable<T>

IComparable<T> 에는 CompareTo()라는 단 하나의 메서드가 정의되어 있다.

<!--more-->

CompareTo()는 현재 객체가 대상 객체보다 작으면 0보다 작은 값, 같으면 0, 크면 0보다 큰 값을 반환한다.

```cs
public struct Customer : IComparable<Customer>, IComparable
{
    private readonly string name;

    public Customer(string name)
    {
        this.name = name;
    }

    // IComparable<Customer> 멤버
    public int CompareTo(Customer other) => name.CompareTo(other.name);
}
```



## (제네릭이 아닌) IComparable

IComparable<T> 를 구현할 때에는 하위호환을 위해 IComparable 도 함께 구현해야 한다.
```cs
	// IComparable 멤버
    int IComparable.CompareTo(object obj)
    {
        if (!(obj is Customer))
            throw new ArgumentException("Argument is not a Customer", "obj");

        Customer otherCustomer = (Customer)obj;

        return this.CompareTo(otherCustomer);
    }
```
IComparable 은 박싱/언박싱이 필요하므로 상당한 성능 비용이 필요하다.

IComparable.CompareTo()를 호출 하기 위해서는 IComparable로 명시적인 형변환이 필요하다.

{% include ad-contents.html %}

## 관계 연산자
표준 관계 연산자를 오버로딩하여 CompareTo() 메서드를 사용하도록 할 수 있다.
```cs
	// 관계 연산자
    public static bool operator < (Customer left, Customer right) =>
        left.CompareTo(right) < 0;
    public static bool operator <= (Customer left, Customer right) =>
        left.CompareTo(right) <= 0;
    public static bool operator > (Customer left, Customer right) =>
        left.CompareTo(right) > 0;
    public static bool operator >= (Customer left, Customer right) =>
        left.CompareTo(right) >= 0;
```

## Comparison<T>
타입에 관한 기본적인 선후 관계 이외에 추가적인  기준으로 정렬이 필요할 경우 Comparison<T>를 사용할 수 있다.
```cs
	public static Comparison<Customer> CompareByRevenue =>
        (left, right) => left.revenue.CompareTo(right.revenue);
```



## IComparer<T>
IComparer<T>를 사용해서 추가적인 정렬 기준을 만들 수도 있다. (제네릭이 생기기 전부터 IComparer가 사용되었었다)
```cs
	private class RevenueComparer : IComparer<Customer>
    {
        // IComparer<Customer> 멤버
        int IComparer<Customer>.Compare(Customer left, Customer right) =>
            left.revenue.CompareTo(right.revenue);
    }

    private static Lazy<RevenueComparer> revComp =
        new Lazy<RevenueComparer>(() => new RevenueComparer());
    
    public static IComparer<Customer> RevenueCompare => revComp.Value;
```

## Reference
* [Comparison<T> 대리자](https://docs.microsoft.com/ko-kr/dotnet/api/system.comparison-1?view=net-6.0){:target="_blank"}
* [IComparer<T> Interface](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1?view=net-6.0){:target="_blank"}
* [C#의 역사](https://docs.microsoft.com/ko-kr/dotnet/csharp/whats-new/csharp-version-history){:target="_blank"}
* [닷넷 프레임워크 버전 역사](https://ko.wikipedia.org/wiki/%EB%8B%B7%EB%84%B7_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_%EB%B2%84%EC%A0%84_%EC%97%AD%EC%82%AC){:target="_blank"}
* [Lazy<T> 클래스](https://docs.microsoft.com/ko-kr/dotnet/api/system.lazy-1?view=net-6.0){:target="_blank"}