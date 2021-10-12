---
title: "[프로그래머스] 합승 택시 요금 (카카오 기출)"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-04-26T21:09:30+09:00
categories:
  - Algorithm
tags:
  - Algorithm
  - Kakao
  - Programmers
permalink: /kakao/taxi/
---
---

## 문제

프로그래머스 2021 KAKAO BLIND RECRUITMENT 기출문제

* [합승 택시 요금](https://programmers.co.kr/learn/courses/30/lessons/72413){:target="_blank"}
<!--more-->


## 풀이

A, B, S 세개의 지점에서 모든 지점으로의 최단거리를 다익스트라(Dijkstra) 알고리즘을 이용하여 구한다.

세 지점으로부터 최단거리의 합이 가장 작은 하나의 지점을 찾아 합승 종료 지점으로 한다.

## 코드 [GitHub](https://github.com/unionyy/algorithm/blob/main/kakao/2021-recruitment-taxi.cpp){:target="_blank"}

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

const int INF = 10000000;
int map[200][200];

void Dijk(int start, int n, vector<int>& dis) {
    dis[start] = 0;
    priority_queue<pair<int, int>> pq;
    pq.push(make_pair(start, 0));
    
    while(!pq.empty()) {
        int next = pq.top().first;
        int cdis = - pq.top().second;
        pq.pop();
        
        if(cdis > dis[next]) continue;
        
        for(int i = 0; i < n; i++) {
            int ndis = cdis + map[i][next];
            if(dis[i] > ndis) {
                dis[i] = ndis;
                pq.push(make_pair(i, - ndis));
            }
        }
    }
    return;
}

int solution(int n, int s, int a, int b, vector<vector<int>> fares) {
    int answer = INF;
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            map[i][j] = INF;
            if(i == j) {
                map[i][j] = 0;
            }
        }
    }
    
    for(auto fare : fares) {
        int r = fare[0] - 1;
        int c = fare[1] - 1;
        int dis = fare[2];
        map[r][c] = dis;
        map[c][r] = dis;
    }
    
    vector<vector<int>> dis(3, vector<int>(n, INF));
    Dijk(s - 1, n, dis[0]);
    Dijk(a - 1, n, dis[1]);
    Dijk(b - 1, n, dis[2]);
    
    for(int i = 0; i < n; i++) {
        int sum = 0;
        for(auto elem : dis) {
            sum += elem[i];
        }
        if(sum < answer) answer = sum;
    }
    
    return answer;
}
```

## Reference

* [합승 택시 요금](https://programmers.co.kr/learn/courses/30/lessons/72413){:target="_blank"}
