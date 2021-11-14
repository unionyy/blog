---
title: "삼성전자 SW 역량테스트 B형 후기 3 (두번째 테스트)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-11-05T17:15:30+09:00
categories:
  - Study
tags:
  - Review
  - Algorithm
  - Samsung
permalink: /study/samsungb3/
---
---

이전 글에 이어지는 삼성전자 SW 알고리즘 역량테스트 B형 취득 후기입니다. 이번 글에서는 2번의 시험 중 두번째 시험을 치르면서 느낀 점을 정리해보았습니다.
<!--more-->

## 준비
첫번째 시험에서 떨어졌다고 생각을 하고 두번째 시험은 더 열심히 준비했습니다.

배웠던 알고리즘들을 다시 한번 살펴보고 제가 구현했던 코드도 다시 읽어보았습니다.

실전 문제도 시간을 체크하면서 4문제 더 풀었습니다.

## 8월 27일 두번째 시험
시험 당일 오전에 첫번째 시험의 결과가 메일로 안내되었습니다. 저는 예상한대로 통과하지 못했습니다. 첫시험 결과를 알고 두번째 시험을 봤기 때문에 더 열심히 볼 수 있었던 것 같습니다.

이번에는 시험에 진지하게 임했고, 메모할 종이도 챙겼습니다.

문제를 처음 봤을 때는 조금 당황했습니다. 생각치도 못했던 생소한 유형의 문제가 나왔기 때문입니다.

첫시험에서 제대로 풀어야했다고 후회하다가 마음을 다잡았습니다. 나름 철저히 준비했기 때문에, 제가 풀지 못할 문제는 나오지 않을 것이라고 생각했습니다. 내가 못풀면 아무도 못풀 것이라는 마음으로 자신감을 갖고 문제를 풀기 시작했습니다.

### 아이디어 도출 및 설계
처음 아이디어를 떠올리는 과정이 제일 어려웠습니다. 어떤 자료구조와 알고리즘을 사용할 수 있는지 하나씩 따져보았습니다. 결국 트리 + DP로 충분히 구현할 수 있겠다는 생각이 떠올랐습니다.

떠올린 아이디어를 실제로 구현할 수 있도록 구체화하여 코드를 설계했습니다. 구체적인 알고리즘과 노드 구조 등을 철저히 설계하였습니다.

### 구현
구현은 설계한 대로 수월하게 수행하였습니다. 구현 도중에 구조를 조금씩 신중하게 변경하였습니다. 첫번째 시험 때의 실수를 반복하지 않도록, 구조를 변경할 때에는 또다른 문제가 발생하지 않는지 꼭 체크하였습니다.

### 디버깅
구현을 모두 마친 뒤에는 테스트해보면서 디버깅을 진행했습니다. 오타 등 자잘한 버그 외에 큰 문제는 없었습니다.

### 최적화
테스트 결과 실행 시간이 꽤 괜찮게 나왔습니다. 그래도 남은 시간동안 최적화를 진행하였습니다.

메인 함수를 수정하여 로그를 찍어보면서 불필요한 연산을 제거하였습니다. 또한 트리에서 중복된 노드를 제거하였습니다.

시험이 종료될 때까지 최적화를 진행하였고, 그 결과 4-5배 정도 빠른 실행시간을 얻을 수 있었습니다. 특수한 버그나 오류가 없으면 무난히 통과할 수 있을 만한 결과를 얻었습니다.

## 시험 결과
8월 27일에 치른 두번째 테스트에 통과하여 삼성전자 SW 알고리즘 역량 테스트 Level B(Pro) 를 취득하였습니다.