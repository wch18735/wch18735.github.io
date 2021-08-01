---
title: "[Coding Test] 진용이네 주차타워 (SWEA: 9280)"
excerpt: "priority queue, queue in cpp"
date: 2021-08-01
layout: single
classes: wide
category:
    - coding test
tag:
    - cpp
    - SWEA
    - dynamic programming 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 진용이네 주차타워 (SWEA: 9280)

이 문제는 전형적인 큐 활용 문제입니다. 아쉽게 **python**은 지원하지 않아 **C++**로 풀었습니다. 어려운 문제이기도 했고, 현재 연수과정에 있어 손이 많이 굳은 느낌이어서 푸는데 시간이 조금 걸렸던 문제입니다.

## 문제

문제를 요약하면 다음과 같습니다.

- 차가 주차장에 도착하면, 비어있는 주차 공간이 있는지 검사
- 비어있는 공간이 없다면, 빈 공간이 생길 때까지 차량을 입구에서 기다리게 함
- 비어있는 공간이 있다면 곧바로 주차를 시킴

이때, 주차 공간 선택 기준은 주차 가능한 공간 중 **번호가 가장 작은 곳** 입니다. 여기에 대기하는 운전자들은 새치기를 하지 않는 조건이 붙습니다.

주차 요금은 차량의 무게와 주차 공간마다 따로 책정된 단위 무게당 금액을 곱한 가격입니다. 즉, 이용시간은 고려하지 않습니다. 

예를들어, 1번 주차장의 무게당 가격이 100원이고 차량의 무게가 5라면 주차가격은 500원이 됩니다.

## 입력

테스트 케이스 입력을 지운 예시입니다.

```
3 4
2
3
5
2
1
3
8
3
2
-3
1
4
-4
-2
-1
```

## 출력

```
#1 53
```

## 팁

이 문제에서 팁은 `#include <queue>` 라이브러리를 잘 활용하는 것입니다. **cpp**로 코딩테스트를 보시는 분들께서는 아래 사항을 체화시키는 것을 추천합니다.

- `priority_queue<Type, Type_Container, Compare>`
- `queue<Type>`

각각의 가장 간단한 예제로는 아래가 될 수 있습니다. 저는 위 사항보다는 아래 예제를 이용해 역으로 위를 기억해내곤 했습니다.

- `priority_queue<int, vector<int>, greater<int>> pq`
- `queue<int> custom_que`

이 두 가지만 잘 활용할 줄 안다면 **FIFO** 문제를 해결하는데 큰 도움을 받을 수 있습니다.

## 소스코드

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>
 
using namespace std;
 
int park_costs_per_weight[101];
int car_weights[2001];
int parked[2001];
 
int main(void)
{
    int TC;
    cin >> TC;
     
    // N: Parking spaces
    // M: Number of cars
    // CAR: Current car
    int N, M, CAR;
    int parking_space;
 
    // priority_queue<T, container, comparing>
    priority_queue<int, vector<int>, greater<int>> parking_spaces;
 
    // waiting queue
    queue<int> waiting_queue;
 
    // answer
    int total_costs = 0;
 
    for (int test_case = 1; test_case <= TC; test_case++)
    {
        cin >> N >> M;
 
        for (int i = 1; i <= N; i++) cin >> park_costs_per_weight[i];
        for (int i = 1; i <= M; i++) cin >> car_weights[i];
         
        // parking spaces
        for (int i = 1; i <= N; i++) parking_spaces.push(i);
 
        for (int i = 0; i < 2 * M; i++)
        {
            // input current car
            cin >> CAR;
 
            // check in / out
            if (CAR > 0)
            {
                // check possible parking space
                if (parking_spaces.empty()) waiting_queue.push(CAR);
                else
                {
                    parking_space = parking_spaces.top();
                    parking_spaces.pop();
 
                    // check and sum cost
                    parked[CAR] = parking_space;
                    total_costs += car_weights[CAR] * park_costs_per_weight[parking_space];
                }
            }
            else
            {
                // park out
                CAR = abs(CAR);
                parking_space = parked[CAR];
                parking_spaces.push(parking_space);
 
                // until waiting nothing or parking spaces full
                while (waiting_queue.empty() == false && parking_spaces.empty() == false)
                {
                    CAR = waiting_queue.front();
                    waiting_queue.pop();
 
                    parking_space = parking_spaces.top();
                    parking_spaces.pop();
 
                    parked[CAR] = parking_space;
                    total_costs += car_weights[CAR] * park_costs_per_weight[parking_space];
                }
            }
        }
 
        cout << '#' << test_case << ' ' << total_costs << '\n';
         
        // initialize
        while (!parking_spaces.empty()) parking_spaces.pop();
        total_costs = 0;
    }
 
    return 0;
}
```