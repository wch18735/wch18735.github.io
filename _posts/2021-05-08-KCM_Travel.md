---
title: "[Algorithm Problem] KCM Travel (BAEKJOON: 10217번)"
excerpt: "Dijkstra and Dynamic Programming"
date: 2021-05-08
layout: single
classes: wide
category:
    - algorithm
tag:
    - C++
    - Python
    - Dijkstra
    - Dynamic Programming
    - Baekjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# KCM Travel

하루 종일, 정말 하루 종일 이것만 푼 것 같았는데 답은 결국 **C++** 이었습니다.
![history](/_img/2021-05-08/solve_history.jpg){: .align-center}

간단하진 않았습니다. 다익스트라 알고리즘에 **해당 노드에 도달했을 때, 최소 비용이 아닌 경우**를 제외하는 다이나믹 프로그래밍 개념을 적용해 풀었습니다. 오랜만에 **C++**를 잡아보니 어색한 것이 많았습니다.  
  
### priority_queue & tuple
**priority_queue\<자료형,컨테이너,비교기준\>** 을 통해 다익스트라 알고리즘에서 후보들 중 가장 짧은 노드를 찾는 우선순위 큐를 만들었습니다. 여기에 한 가지 더 활용된 것이 **\<tuple\>** 입니다. **C++**에서 문제를 풀 때, **\<pair\>**를 사용하여 자료형을 만들 때, 조금 복잡해지는 경우가 있습니다. 이럴 때 튜플 자료형을 사용하면 조금 더 접근이 쉬워질 것입니다.

## C++ 소스코드

기억하면 좋을 스킬은 아래와 같습니다.
- **get\<index\>(tuple)**
- **const int MAXNUM = 123456789**
- **fill(&array[0][0], &array[0][0] + sizeof(array) / sizeof(array[0][0]), integer)**

``` cpp
#include <iostream>
#include <tuple>
#include <algorithm>
#include <queue>
#include <vector>

using namespace std;

// 자료형 정의
typedef tuple<int, int, int> next_info;
vector<next_info> tickets[10001];
int dp[101][10001];
const int MAXNUM = 123456789;

int main(int argc, char* argv[])
{
	int T;
	cin >> T;

	for (int test_case = 1; test_case <= T; test_case++)
	{
		int N, M, K;
		cin >> N >> M >> K;

		// 초기화
		fill(&dp[0][0], &dp[0][0] + sizeof(dp) / sizeof(dp[0][0]), MAXNUM);
		for (int i = 1; i <= M; i++) tickets[i].clear();

		// 인접 노드 입력
		for (int i = 0; i < K; i++)
		{
			int u, v, c, d;
			cin >> u >> v >> c >> d;
			tickets[u].push_back({ v,c,d });
		}

		// dijkstra 위한 큐
		priority_queue<next_info, vector<next_info>, greater<next_info>> pq;

		// dp 초기화 (total time, total cost, node number)
		dp[1][0] = 0;
		pq.push({ 0, 0, 1 });

		while (!pq.empty())
		{
			int current_time = get<0>(pq.top());
			int current_cost = get<1>(pq.top());
			int current_node = get<2>(pq.top());
			pq.pop();

			for (int adj_idx = 0; adj_idx < (int)(tickets[current_node].size()); adj_idx++)
			{
				next_info ni = tickets[current_node][adj_idx];
				
				int next_node = get<0>(ni);
				int next_cost = current_cost + get<1>(ni);
				int next_time = current_time + get<2>(ni);

				// 가능한 수량을 초과할 때
				if (next_cost > M) continue;

				// dp[next_node][next_cost] > next_time 이 아닐 때
				if (dp[next_node][next_cost] <= next_time) continue;

				// dp 업데이트
				dp[next_node][next_cost] = next_time;
				pq.push({ next_time, next_cost, next_node });
			}
		}

		// 정답 확인
		int answer = MAXNUM;
		for (int i = 1; i <= M; i++) answer = min(answer, dp[N][i]);

		if (answer == MAXNUM) cout << "Poor KCM\n";
		else cout << answer << '\n';
	}

	return 0;
}
```

## Python 소스코드
같은 로직인데 안 풀리니 너무 아쉽네요.

``` python
from heapq import heappop, heappush

# test case
T = int(input())

for test_case in range(1, T + 1):
    # 노드 수, 총 지원비용, 간선 개수
    N, M, K = map(int, input().split(' '))

    # 인접 노드
    adj_node_list = [[] for _ in range(N + 1)]

    # [도착노드][사용한 비용] <- 걸린 시간
    dp = [[float('inf') for i in range(M + 1)] for j in range(N + 1)]

    for _ in range(K):
        # 출발, 도착, 비용
        u, v, c, d = map(int, input().split(' '))

        # 인접 노드 추가
        adj_node_list[u].append((v, c, d))

    # 힙 선언 및 초기화
    # pq <- (time, cost, node_number)
    HEAP = []
    heappush(HEAP, (0, 0, 1))
    dp[1][0] = 0

    while HEAP:
        # HEAP Q 중 우선순위 가장 높은 것
        current_t, current_c, current_n = heappop(HEAP)

        # 인접 노드 검색
        for next_n, interval_c, interval_t in adj_node_list[current_n]:
            # 다음 노드에 해당하는 비용과 시간
            next_c = current_c + interval_c
            next_t = current_t + interval_t

            # 지원금 초과 검사
            if next_c > M:
                continue

            # 다음 도달할 위치 값과 시간 비교
            if dp[next_n][next_c] <= next_t:
                continue

            # 최소 결과 값 저장
            dp[next_n][next_c] = next_t
            heappush(HEAP, (next_t, next_c, next_n))

    if min(dp[N][1:]) == float('inf'):
        print('Poor KCM')
    else:
        print(min(dp[N][1:]))
```