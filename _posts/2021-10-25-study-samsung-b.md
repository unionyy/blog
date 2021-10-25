---
title: "삼성전자 SW 역량테스트 B형 후기 1"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-10-25T16:05:30+09:00
categories:
  - Study
tags:
  - Review
  - Algorithm
  - Samsung
permalink: /study/samsungb/
---
---

삼성전자 S/W 알고리즘 특강을 수강하여 운좋게 삼성전자 SW 알고리즘 역량테스트 B형(pro) 을 치를 기회를 얻었습니다. 2회의 기회가 주어졌고 저는 두번째 테스트에서 통과하여 B형을 취득하였습니다.
<!--more-->

이 글은 제가 B형 테스트를 준비하고 2번의 테스트를 치르면서 느낀점들을 바탕으로 작성한 글입니다. 참고 정도로 보시기를 추천드립니다.

글을 쓰다보니 생각보다 길어져서 이번 포스트에서는 시험 준비 과정에 대한 내용을 다루고 시험을 친 후기는 다음 포스트로 작성하겠습니다.

## A형 테스트와 비교

### 제약사항
지원 언어는 C/C++/Java/Python 으로 같고 라이브러리 또한 사용 가능으로 같습니다. (이전에는 B형의 경우 Python 사용이 불가능하고 라이브러리 사용이 제한되었으나 이번 시험부터 변경되었다고 합니다)

### 문제 수 & 시간
* A형: 3시간 2문제
* B형: 4시간 1문제

A형 시험은 문제를 잘 이해하고 주어진 시간 내에 구현할 수 있는가를 확인하는 테스트라고 느꼈습니다. 문제를 잘 읽고 그대로 구현하기만 하면 됩니다.

웬만해선 시간초과나 메모리 초과가 발생할 일이 없을겁니다. (DFS, BFS, DP 등 몇몇 알고리즘만 능숙하게 구현할 수 있으면 통과할 수 있다고 봅니다)

반면 B형은 주어진 문제를 얼마나 효율적으로 해결할 수 있는가를 봅니다. 문제 자체는 A형보다 간단해서 문제 이해하기는 쉽습니다.

그러나 제약사항으로 주어지는 사이즈, 횟수 등이 엄청나게 커서 A형처럼 생각나는 대로 구현하기만 하면 시간초과나 메모리 초과로 절대 통과할 수 없습니다.

여러가지 자료구조나 알고리즘에 대한 이해, 그리고 원하는 대로 변형시키고 능숙하게 구현할 수 있는 능력이 필요합니다.

{% include ad-contents.html %}

## STL 사용
이제 B형 테스트에서도 A형 테스트와 마찬가지로 STL을 사용해도 된다고 합니다. 그러나 저는 STL을 사용하지 않았습니다.

단순히 STL을 사용하기만 해서 풀 수 있는 문제가 출제되지는 않습니다. 자료구조나 알고리즘을 문제에 맞게 변형하거나 결합시켜야하는 문제들이 출제되므로 저는 오히려 STL을 사용하지 않고 직접 구현하고 변형시키면서 푸는 것이 효율적이라고 느꼈습니다.

STL을 원하는 대로 능숙하게 사용할 수 있고, 어떻게 동작하는지 잘 이해하고 있는 경우가 아닌 이상 STL 없이 푸는 것을 추천합니다.

그리고 [SW Expert Academy](https://swexpertacademy.com/){:target="_blank"} 에 올라온 연습문제 대부분은 STL을 사용할 수 없을 겁니다. (확실하진 않습니다. 아마 그럴 겁니다!)

1문제를 4시간 동안 푸는 시험이니, STL을 사용하지 않아도 결코 시간이 부족하지 않습니다.

## 준비
저는 삼성전자 알고리즘 역량강화 과정에 참여하여 공부하였습니다. 자세한 내용은 이전에 포스팅했던 글을 참고하세요.
* [삼성전자 2021 하계 대학생 S/W 알고리즘 역량강화 특강 수료 후기](/study/algorithm2021/){:target="_blank"}

## 알고리즘 공부
B형 테스트를 통과하기 위해선 여러 알고리즘에 대한 이해가 필요합니다.

그러나 알고리즘이라고 해봤자 생각보다 복잡하지 않습니다. 대부분 알고리즘 강의에서 배웠던 내용들이고 개념을 이해하고 구현 연습을 하면 어렵지 않습니다. (제가 까다롭다고 느낀 알고리즘은 KMP, LCA 정도입니다)

### 공부해볼만한 키워드
Array, Linked List, Bitwise 연산, Greedy, 완전탐색(Brute-Force), BFS, DFS, DP, 분할정복, Sort, Binary Search, Graph, Dijkstra, Tree, LCA, Heap, Trie, KMP, Hash

알고리즘 공부를 하면서 자료구조와 알고리즘의 특징이나 시간복잡도에 대한 생각을 많이 하고 잘 이해해야합니다. 위의 키워드들을 다 이해하고 구현할 수 있을 정도면 충분합니다.

{% include ad-contents.html %}

## 문제 풀이 연습

알고리즘 공부가 잘 됐다면 관건은 문제를 보고 알맞은 알고리즘 & 자료구조를 떠올리고 변형 및 적용하는 것입니다. 여러 문제들을 풀어보고 다른 사람의 풀이를 보면서 생각의 흐름을 익히는 것이 중요합니다.

문제를 보고 아이디어가 바로바로 떠오르면 좋겠지만 쉽지 않습니다. 문제를 풀어보면서 특정 자료구조 & 알고리즘을 사용한 이유를 생각해보고 문제의 제약사항 등을 보면서 차근차근 아이디어를 떠올리는 연습을 해야합니다.

문제는 아래의 흐름대로 풉니다. 무작정 푸는 것보다 효율적이며 시간관리에도 용이합니다.

문제 이해 -> 아이디어 도출 -> 설계 -> 구현 -> 디버깅 -> 최적화

### 설계
설계과정까지 30분, 넉넉잡아 1시간까지도 줍니다. 설계를 탄탄히 할수록 이후 과정이 수월해지므로 설계에서 시간을 오래 소모했다고 조급해할 필요 없습니다.

여러가지 시나리오를 생각해보며 효율적인 알고리즘을 선택해야합니다. 특히 엣지 케이스를 고려하여 특정 상황에서 큰 시간이 소모될 경우가 없는지 생각해보아야합니다. (edge case와 bad case는 다릅니다. 저격성 케이스는 주어지지 않는다고 봅니다.)

### 구현
설계를 제대로 했다면 구현은 어렵지 않을 겁니다. 설계한대로 코드만 작성하면됩니다. 그러나 설계할때 생각지 못했던 문제가 발생할 때가 많습니다. 이때는 구현을 잠시 멈추고 다시 설계를 신중히 수정하는 것이 좋습니다.

구현하면서 그때그때 되는 대로 설계를 수정하게 되면 또 새로운 문제가 발생할 확률이 높고 설계할 때 고려했던 사항들이 무시되는 상황이 발생할 수 있습니다. 조급해하지 말고 신중히 다시 설계하는게 오히려 효율적입니다.

### 디버깅
구현을 끝냈으면 테스트 케이스를 실행해보면서 디버깅을 합니다. 이때 main 함수나 테스트 셋을 수정해보면서 버그를 찾고 엣지 케이스에서 잘 동작하는지 확인합니다.

B형 테스트는 main 함수는 수정할 수 없고 solution 함수만 구현하면 됩니다. 그러나 디버깅을 하면서 main 함수를 수정하여 로그를 찍는 등의 과정이 필요하므로 문제를 풀면서 main함수를 수정해보는 연습도 해보면 도움이 됩니다.

### 최적화
디버깅까지 마쳤을 때 시간이 남는다면 최적화를 해줍니다. 중복된 연산, 불필요한 연산 등을 찾아 최적화해줍니다. 시간이 얼마남지 않았다면 시간초과가 나지 않는 이상 무리하게 최적화 작업을 하는 것은 추천하지 않습니다. 마지막에 급하게 제출하면 예상치못한 버그가 발생할 수 있기 때문입니다.