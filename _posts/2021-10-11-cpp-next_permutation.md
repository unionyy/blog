---
title: "[C++ STL] 순열과 조합 (next_permutation)"
excerpt: next_permutation 함수는 위와 같이 사용하고 파라미터로 보내진 Iterator 범위 내의 원소들을 다음 경우의 수 배열로 만들어 줍니다. 이 때, 오름차순에서 내림차순으로 가는 경우의 수를 고려합니다. 즉, 오름차순으로 정렬된 배열에 next_permutation 을 계속 사용하면 모든 경우의 수를 거쳐 결국 내림차순으로 정렬됩니다.
layout: single
date: 2021-10-11T00:05:30+09:00
categories:
  - C++
tags:
  - C++
  - STL
permalink: /cpp/next_permutation/
---
---
{% include ad-contents.html %}

코딩테스트를 보는 도중에 순열을 만들어야하는 상황이 있었습니다. 시간은 15분이 남아있었고, **next_permutation** 함수 사용법이 정확히 기억나지 않았습니다. (어느 라이브러리에 있는지 생각이 나지 않았습니다 ㅠ)

Bitwise 연산으로 조합은 구현해본 적이 있었으나 순열은 직접 구현해본 적이 없었습니다. 남은 10분여의 시간동안 순열을 구하는 알고리즘을 생각해내고 구현을 하기란 쉽지 않았고 결국 다 푼 문제를 순열을 만들지 못해 틀리게 되었습니다.

이번 기회에 **next_permutation** 함수를 제대로 정리하고 기억해두려합니다. 또한 **next_permutaion** 함수가 어떻게 생겼는지 살펴보고 순열을 직접 구현할 수 있도록 했습니다.

## next_permutation 사용법

* [std::next_permutation](https://en.cppreference.com/w/cpp/algorithm/next_permutation){:target="_blank"}

```cpp
next_permutation(Arr.begin(), Arr.end());
```
**next_permutation** 함수는 위와 같이 사용하고 파라미터로 보내진 Iterator 범위 내의 원소들을 다음 경우의 수 배열로 만들어 줍니다. 이 때, 오름차순에서 내림차순으로 가는 경우의 수를 고려합니다.

즉, 오름차순으로 정렬된 배열에 **next_permutation** 을 계속 사용하면 모든 경우의 수를 거쳐 결국 내림차순으로 정렬됩니다.

**next_permutation** 함수는 bool 형식의 값을 반환합니다. 함수가 제대로 작동했으면 true 를 리턴하고 이미 배열이 내림차순으로 정렬되어있을 경우에는 false 를 반환합니다.

## 예시
다음은 레퍼런스 페이지에서 제공하는 예시입니다.
```cpp
#include <algorithm>
#include <string>
#include <iostream>
 
int main()
{
    std::string s = "aba";
    std::sort(s.begin(), s.end());
    do {
        std::cout << s << '\n';
    } while(std::next_permutation(s.begin(), s.end()));
}

/* OUTPUT */
/* aab    */
/* aba    */
/* baa    */
```

{% include ad-contents.html %}

## next_permutation 함수 살펴보기

**next_permutation** 함수는 이렇게 생겼습니다.
```cpp
template<class BidirIt>
bool next_permutation(BidirIt first, BidirIt last)
{
    if (first == last) return false;
    BidirIt i = last;
    if (first == --i) return false;
 
    while (true) {
        BidirIt i1, i2;
 
        i1 = i;
        if (*--i < *i1) {
            i2 = last;
            while (!(*i < *--i2))
                ;
            std::iter_swap(i, i2);
            std::reverse(i1, last);
            return true;
        }
        if (i == first) {
            std::reverse(first, last);
            return false;
        }
    }
}
```
위 코드의 알고리즘은 이렇습니다.

1. 맨 뒤에서부터 인접한 두개의 수를 비교한다. (뒤에서부터 내림차순이 아닌 위치를 찾기 위함)

2. 앞의 수(A)가 작을 경우 비교를 멈춘다. (A 뒤에 있는 수들은 내림차순으로 정렬되어있는 상태)

3. 맨 뒤에서부터 A와 비교하여 A보다 커지는 지점(B)을 찾는다.

4. A와 B를 스왑한다. (여전히 뒤의 수들은 내림차순)

5. 뒤의 수들을 반전시킨다. (내림차순에서 오름차순으로 만든다)

생각보다 간단하고 깔끔한 알고리즘으로 구현되어 있어서 놀랐습니다. 그래서 코딩 테스트 도중에 구현하지 못하였던 것이 더욱 아쉬웠습니다. 이제 순열을 구현하는 방법은 절대 잊지 않을 것 같습니다!

## 조합으로 사용하기
**next_permutation** 함수는 조합을 구현할 때에도 사용할 수 있습니다.

0과 1으로만 이루어진 배열(0이 1보다 앞에 있어야함)을 만들어서 **next_permutation** 함수를 연속적으로 호출하면 000...0111...1 부터 111...1000...1 까지 모든 경우의 수를 얻을 수 있습니다.

예시
```cpp
#include <algorithm>
#include <iostream>
 
int main()
{
    int Arr[5] = {0, 0, 1, 1, 1};
    std::sort(Arr, Arr+5);
    do {
        for(int elem : Arr) {
            std::cout << elem;
        }
        std::cout << '\n';
    } while(std::next_permutation(Arr, Arr+5));
}

// OUTPUT
// 00111
// 01011
// 01101
// 01110
// 10011
// 10101
// 10110
// 11001
// 11010
// 11100
```

## Reference
* [std::next_permutation](https://en.cppreference.com/w/cpp/algorithm/next_permutation){:target="_blank"}

{% include ad-contents.html %}