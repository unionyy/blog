---
title: "[Effective C#] 아이템 19: 런타임에 타입을 확인하여 최적의 알고리즘을 사용하라"
excerpt_separator: "<!--more-->"
layout: single
date: 2022-04-27T23:16:30+09:00
categories:
  - C#
tags:
  - C#
  - Effective C#
permalink: /effective-cs-item19/
---
---
제네릭의 인스턴스화는 런타임의 타입을 고려하지 않고 컴파일 타임의 타입만을 고려한다.
<!--more-->

따라서 제네릭을 사용하면 구체적인 타입이 주는 장점을 잃게 되고, 타입별로 최적화된 알고리즘을 사용할 수 없게 된다.

밑의 코드는 IEnumerable<T>와 IEnumerator<T>를 구현한 코드로, 특정 시퀀스를 역순으로 순회하는 코드이다.

```cs
	public sealed class ReverseEnumerable<T> : IEnumerable<T>
    {
        public class ReverseEnumerator : IEnumerator<T>
        {
            int currentIndex;
            IList<T> collection;

            public ReverseEnumerator(IList<T> srcCollection)
            {
                collection = srcCollection;
                currentIndex = collection.Count;
            }

            // IEnumerator<T> 멤버
            public T Current => collection[currentIndex];

            // IDisposable 멤버
            public void Dispose()
            {
                // 생략
            }

            // IEnumerator 멤버
            object System.Collections.IEnumerator.Current => this.Current;
            public bool MoveNext() => --currentIndex >= 0;
            public void Reset() => currentIndex = collection.Count;
        }


        IEnumerable<T> sourceSequence;
        IList<T> originalSequence;

        public ReverseEnumerable(IEnumerable<T> sequence)
        {
            sourceSequence = sequence;
        }

        // IEnumerable<T> 멤버
        public IEnumerator<T> GetEnumerator()
        {
            if(originalSequence == null)
            {
                originalSequence = new List<T>();
                foreach (T item in sourceSequence)
                    originalSequence.Add(item);
            }
            return new ReverseEnumerator(originalSequence);
        }

        // IEnumerable 멤버
        System.Collections.IEnumerator
            System.Collections.IEnumerable.GetEnumerator() =>
                this.GetEnumerator();
    }
```
ReverseEnumerable<T>를 호출하면 List에 해당 시퀀스를 전부 옮기고 리스트를 역순으로 순회하게 된다.

그러나 매개변수로 전달한 IEnumerable<T>가 IList<T>를 구현했다면 리스트에 시퀀스를 옮기지 않고 랜덤 엑세스가 가능하다.

따라서 IList<T>를 구현한 매개변수에 한해서 좀 더 효율적인 코드를 작성할 수 있다. (복사 과정이 생략되므로)

```cs
        public ReverseEnumerable(IEnumerable<T> sequence)
        {
            sourceSequence = sequence;

            originalSequence = sequence as IList<T>;
        }

		public ReverseEnumerable(IList<T> sequence)
        {
            sourceSequence = sequence;
            originalSequence = sequence;
        }
```
ReverseEnumerable<T>의 생성자를 다음과 같이 변경하면, 매개변수가 IList<T>일 경우에는 복사가 생략된다.

(IList<T>로 생성자 오버로드를 해도 되지만, 런타임에 IList<T> 타입의 객체가 컴파일 타임에는 IEnumerable<T> 타입으로 간주되는 경우가 있으므로 런타임에 타입을 확인하는 코드는 필요하다)

이외에도 입력 시퀀스가 ICollection<T> 만을 구현한 경우 Count 속성을 활용하여 저장소 공간을 미리 초기화하도록 코드를 개선할 수 있다.

또한 string의 경우 IList<char>를 구현한 것은 아니지만 그와 비슷하게 랜덤 엑세스가 가능한데, 이를 활용하도록 코드를 개선할 수도 있다.

이와 같이 제네릭을 사용하면서 각 타입들의 고유 특성을 활용하면 재사용성이 높으면서도 개별 타입에 최적화된 코드를 작성할 수 있다.

## Reference
* [IEnumerable 인터페이스](https://docs.microsoft.com/ko-kr/dotnet/api/system.collections.ienumerable?view=net-6.0){:target="_blank"}