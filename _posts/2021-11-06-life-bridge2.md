---
title: "오징어 게임 징검다리 건너기 참가자별 성공 확률 2 (파이썬 테스트)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-11-14T17:08:30+09:00
categories:
  - Life
tags:
  - Squid Game
  - Python
permalink: /life/bridge2/
---
---

이전 글에서 수학적으로 계산했던 오징어 게임 징검다리 건너기 참가자별 성공 확률을 코드로 구현하여 테스트해보았습니다.
<!--more-->

## 게임 함수
게임을 1회 진행하는 함수입니다. level 파라미터는 징검다리의 개수, printBridge는 게임이 끝난 후 징검다리의 모양을 프린트할지 여부입니다. 최초로 성공한 플레이어의 번호를 리턴합니다.

```python
import random

def game(level, printBridge):
    res = []
    player = 1
    for i in range(level):
        isBroke = random.randrange(2)
        if isBroke:
            res.append(True)
            player += 1
        else:
            res.append(False)
    
    if printBridge:
        for isBroke in res:
            if isBroke:
                print("X", end=" ")
            else:
                print("O", end=" ")
    
    return player
```

## 게임 1회 진행
![1 game](/assets/post-images/life-bridge2/1.png)

게임을 1회 진행해본 결과 9번 참가자가 최초로 성공했습니다. 16명의 참가자가 있었다면 8명의 참가자가 성공했겠네요. 드라마 오징어게임보다 많이 생존했습니다!

## 게임 100만번 진행
```python
res = []
for i in range(19):
    res.append(0)
for i in range(1000000):
    player = game(18, False)
    res[player - 1] += 1

print(res)
```
게임을 100만번 진행하여 최초로 성공한 참가자를 축적하는 코드입니다.

{% include ad-contents.html %}

## 플랏
```python
import matplotlib.pyplot as plt

prob = []
for i in range(19):
    prob.append(res[i] / 1000000)
plt.plot(range(1, 20), prob, "o")
```
위의 결과를 확률로 계산하여 플랏했습니다.

![plot1](/assets/post-images/life-bridge2/plot1.png)

예상대로 이항분포 꼴의 그래프를 얻었습니다!

## 각 참가자별 성공 확률
위의 확률을 축적하여 각 참가자별 성공 확률을 계산하고 플랏하였습니다.

```python
resAccum = []
resAccum.append(res[0])
for i in range(18):
    resAccum.append(resAccum[i] + res[i+1])

accumProb = []
for i in range(19):
    accumProb.append(resAccum[i] / 1000000)
plt.plot(range(1, 20), accumProb, "o")

for i in range(19):
    print(str(i+1) + "번 참가자: " + str(round(accumProb[i] * 100, 4)) + "%")
```

![plot2](/assets/post-images/life-bridge2/plot2.png)

```
1번 참가자: 0.0003%
2번 참가자: 0.0086%
3번 참가자: 0.0688%
4번 참가자: 0.3781%
5번 참가자: 1.5433%
6번 참가자: 4.8335%
7번 참가자: 11.9258%
8번 참가자: 24.0721%
9번 참가자: 40.7867%
10번 참가자: 59.3759%
11번 참가자: 76.0369%
12번 참가자: 88.1458%
13번 참가자: 95.1891%
14번 참가자: 98.4459%
15번 참가자: 99.6244%
16번 참가자: 99.9353%
17번 참가자: 99.9923%
18번 참가자: 99.9996%
19번 참가자: 100.0%
```
이전 글에서 계산했던 값과 거의 일치하는 결과를 얻었습니다.

## 전체 코드
* [bridge.ipynb](https://github.com/unionyy/laboratory/blob/main/glass-bridge/bridge.ipynb){:target="_blank"}

## Reference
* [01. Matplotlib 기본 사용](https://wikidocs.net/92071){:target="_blank"}
