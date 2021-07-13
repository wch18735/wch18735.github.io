---
title: "[Algorithm Problem] 숨바꼭질 시리즈 (BAEKJOON: 1697, 12851, 13549, 13913번)"
excerpt: "BFS and Dynamic programming"
date: 2021-07-13
layout: single
classes: wide
category:
    - algorithm problem
tag:
    - python
    - Beakjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 숨바꼭질 시리즈

이번에 다룰 포스팅은 숨바꼭질 시리즈입니다.

- [숨바꼭질1 (BAEKJOON: 1697)](https://www.acmicpc.net/problem/1697)
- [숨바꼭질2 (BAEKJOON: 12851)](https://www.acmicpc.net/problem/12851)
- [숨바꼭질3 (BAEKJOON: 13549)](https://www.acmicpc.net/problem/13549)
- [숨바꼭질4 (BAEKJOON: 13913)](https://www.acmicpc.net/problem/13913)

이 문제를 풀면서 **너비 우선 탐색(BFS)** 과 **동적프로그래밍** 을 결합한 풀이 방법을 고민해볼 수 있을 것입니다. 문제의 기본 틀부터 살펴보겠습니다.

수빈이와 동생이 숨바꼭질을 하고 있습니다. 수빈이는 현재 점 **N**$(0 \leq N \leq 100,000)$에 위치하고, 동생은 점 **K**$(0 \leq K \leq 100,000)$ 에 위치합니다.

수빈이는 걷거나 순간이동을 할 수 있습니다. 수빈이의 위치가 $X$일 때 수빈이는 다음과 같이 이동할 수 있습니다.

- 오른쪽으로 걷는 경우: $X → X + 1$
- 왼쪽으로 걷는 경우: $X → X - 1$
- 순간이동 하는 경우: $X → X * 2$

걷거나 순간이동 할 때 시간이 주어지는 경우, 동생을 찾는 최소 시간을 구하는 것이 문제의 핵심입니다.

## 풀이

저는 이 문제에서 중요한 점은 아래 두 가지라고 생각합니다.

- **BFS** 설계
- **DP Cache** 설계

### BFS 설계

먼저 BFS 설계에 관해 알아보겠습니다. **BFS**는 기본적으로 다음과 같은 형태를 띕니다.

```python
dq = deque()
dq.append(초기값)

while bool(dq):
    # 데이터 반환
    # data = dq.popleft()

    # 종료 부분
    # continue

    # 반환 값 이용한 연산
    
    # 조건 확인부
    # dq 추가
```

- **종료 부분:** **Cache**를 이용할 때, 불필요한 연산이 발생하는 것을 막아줍니다.
- **반환 값 이용한 연산:** 덱에서 얻은 값을 이용해 특정 조건을 만족하는지 계산하는 작업, 외부 전역 변수 값을 초기화 하는 작업을 말합니다.
- **조건 확인부:** 조건을 확인하여 만족하는 노드를 덱에 추가하는 작업을 뜻합니다.

조건 확인부에서 <span style="color: #FF2323; font-weight: bold">Cache로 지정한 배열의 범위를 넘어서지 않도록 범위를 체크해주는 것</span>이 **정말정말 중요합니다.** 또한 대기하는 덱에 저장하기 전, 도달하는 시간이 최소 시간보다 늦은 경우를 체크하여 연산을 줄여줍니다.

### Cache 설계

**Cache 설계** 는 어떤 값을 이용해 불필요한 연산을 줄일 수 있을지, 결과값을 활용할 수 있을지 등을 고민하는 영역입니다. 

저는 설계한 BFS 형태에서 인자로 전달하는 값으로 `SECOND`를 정했습니다. 노드의 깊이에 해당하는 값으로 생각할 수 있겠네요.

해당 노드까지 도달하는데 걸린 **시간(초)**이 이전에 이곳을 방문한 시간보다 늦은 경우 더 연산을 할 필요 없습니다. 저는 `positino_cache` 에 도달한 최소 시간을 저장했습니다.

## 소스 코드

그럼 각 문제별 정답 코드를 보며 내용을 확인해보시면 될 것 같습니다.

### 숨바꼭질

```cpp
#include <iostream>
#include <deque>

using namespace std;

#define MAXNUM 200001

int bfs(int pos, int target, int* dp) {
	int Answer = 0;

	deque<pair<int, int>> next_pos; // 위치,count
	
	next_pos.push_back({ pos, 0 });

	while (!next_pos.empty()) {
		int poped = next_pos.front().first;
		int count = next_pos.front().second;
		next_pos.pop_front();

		// target일 경우
		if (poped == target) {
			Answer = count;
			break;
		}

		// target이 아닐 경우

		if (poped * 2 <= MAXNUM) {
			if (dp[poped * 2] > count) {
				next_pos.push_back({ poped * 2, count + 1 });
				dp[poped * 2] = count+1;
			}
		}
		if (poped - 1 >= 0) {
			if (dp[poped - 1] > count) {
				next_pos.push_back({ poped - 1, count + 1 });
				dp[poped - 1] = count + 1;
			}
		}
		if (poped + 1 <= MAXNUM) {
			if (dp[poped + 1] > count) {
				next_pos.push_back({ poped + 1, count + 1 });
				dp[poped + 1] = count;
			}
		}
	}

	return Answer;
}

int main(void) {
	int pos, target;
	cin >> pos >> target;

	int dp[200001];
	fill_n(dp, MAXNUM, MAXNUM);

	int Answer = bfs(pos, target, dp);

	cout << Answer;

	return 0;
}
```

### 숨바꼭질2

```python
from collections import deque

def solution():
    N, K = map(int, input().split(' '))
    
    # 각 위치 값은 도착한 시간
    position_cache = [float('inf') for i in range(100_001)]

    # 최소 시간
    min_sec = float('inf')
    
    # 경우의 수 개수
    answer = 0

    # deque
    dq = deque()
    
    # dq 초기 값 -> (시간, 경우의 수, 수빈 위치)
    dq.append((0, N))
    
    # 탐색
    while bool(dq):
        sec, pos = dq.popleft()
        position_cache[pos] = sec

        if sec > min_sec:
            # 탐색 중인 과정의 시간이 최소 도달시간보다 길 때
            continue
        
        if pos == K:
            # 도달했을 때
            if sec < min_sec:
                # 도착시간이 더 짧을 경우
                min_sec = sec
                answer = 1
                continue
            elif sec == min_sec:
                # 정확한 도착시간일 때
                answer += 1
                continue

        # 다음 위치들
        next_pos_left = pos - 1
        next_pos_right = pos + 1
        next_pos_jump = pos * 2

        if next_pos_left >= 0:
            if position_cache[next_pos_left] > sec:
                dq.append((sec + 1, next_pos_left))

        if next_pos_right <= 100_000:
            if position_cache[next_pos_right] > sec:
                dq.append((sec + 1, next_pos_right))

        if next_pos_jump <= 100_000 and next_pos_right != 0:
            if position_cache[next_pos_jump] > sec:
                dq.append((sec + 1, next_pos_jump))

    print(min_sec)
    print(answer)
        
if __name__=="__main__":
    solution()
```

### 숨바꼭질3

```python
from collections import deque

def solution():
    N, K = map(int, input().split(' '))

    position_cache = [float('inf') for i in range(100_001)]

    dq = deque()
    dq.append((N, 0))   # position, second

    min_sec = float('inf')
    while bool(dq):
        pos, sec = dq.popleft()
        
        # 현재 위치에 대한 값 넣기
        position_cache[pos] = sec

        # 최소 시간보다 길 경우
        if min_sec <= sec:
            continue

        if pos == K:
            min_sec = min(min_sec, sec)
            continue

        # 다음 위치
        next_pos_right = pos + 1
        next_pos_left = pos - 1
        next_pos_jump = pos * 2

        # 다음 위치 탐색에 추가
        if next_pos_left >= 0:
            if sec < position_cache[next_pos_left]:
                dq.append((next_pos_left, sec + 1))

        if next_pos_right <= 100_000:
            if sec < position_cache[next_pos_right]:
                dq.append((next_pos_right, sec + 1))

        if next_pos_jump != 0 and next_pos_jump <= 100_000:
            if sec < position_cache[next_pos_jump]:
                dq.append((next_pos_jump, sec))

    print(min_sec)

if __name__=="__main__":
    solution()
```

### 숨바꼭질4

이 문제를 푸는 키는 **풀이에 사용했던 Cache 값을 사용해서 어떻게 경로를 추적할까?** 를 알아내는 것이었습니다. 나머지 부분은 정말 동일합니다.

저는 도달하는데 걸린 최소 시간이 1초씩 줄어드는 것에 착안해 `현재 위치`로 올 수 있는 `후보 위치`들 중 `현재 시간 - 1`인 값으로 이동하는 방법을 취했습니다.

저는 좌측, 우측, 순간이동 순서로 체크했습니다. 이 체크 순서에 따라 답이 달라질 수 있으니 예제와 답이 다르다하여 고민하지 마시고 제출해보시는 걸 추천드립니다.

```python
from collections import deque

def solution():
    N, K = map(int, input().split(' '))

    # DP Cache: position_cache[pos] -> arriving time
    position_cache = [float('inf') for i in range(100_001)]

    dq = deque()
    dq.append((N, 0))

    min_sec = float('inf')

    while bool(dq):
        pos, sec = dq.popleft()

        if position_cache[pos] > sec:
            position_cache[pos] = sec

        # 늦은 도착시간 없애기
        if min_sec <= sec:
            continue

        # 도착한 경우
        if pos == K:
            min_sec = min(sec, min_sec)
            continue

        # 다음 위치
        next_left = pos - 1
        next_right = pos + 1
        next_jump = pos * 2

        # 좌, 우, 점프 이동
        if next_left >= 0:
            # 왼쪽으로 이동한 경우가 0보다 작은 경우 pass
            if sec < position_cache[next_left]:
                # 다음으로 이동할 위치에 이미 이전 시간에 도달한 경우 pass
                dq.append((next_left, sec + 1))

        if next_right <= 100_000:
            # 오른쪽으로 이동한 경우가 0보다 작은 경우 pass
            if sec < position_cache[next_right]:
                # 다음으로 이동할 위치에 이미 이전 시간에 도달한 경우 pass
                dq.append((next_right, sec + 1))

        if next_jump != 0 and next_jump <= 100_000:
            # 왼쪽으로 이동한 경우가 0보다 작은 경우 pass
            if sec < position_cache[next_jump]:
                # 다음으로 이동할 위치에 이미 이전 시간에 도달한 경우 pass
                dq.append((next_jump, sec + 1))

    # 최소 시간 출력
    print(min_sec)

    # 경로 역 추적
    route = list()

    # traceback second, traceback position
    tb_sec = min_sec - 1
    tb_pos = K

    while True:
        route.append(tb_pos)
        if tb_pos == N:
            break
        
        # 이전 위치
        prev_left = tb_pos - 1
        prev_right = tb_pos + 1
        if tb_pos % 2 != 0:
            prev_jump = -1
        else:
            prev_jump = tb_pos // 2

        if prev_left >= 0:
            if position_cache[prev_left] == tb_sec:
                tb_sec -= 1
                tb_pos = prev_left
                continue

        if prev_right <= 100_000:
            if position_cache[prev_right] == tb_sec:
                tb_sec -= 1
                tb_pos = prev_right
                continue

        if prev_jump != -1:
            if position_cache[prev_jump] == tb_sec:
                tb_sec -= 1
                tb_pos = prev_jump
                continue

    print(*route[::-1])

if __name__=="__main__":
    solution()
```

## 승급!

이 문제 시리즈를 다 푸니 딱 <span style="color: #FFD700; font-weight: bold">골드1</span>로 승급하네요. 조금 더 노력해서 올해 목표는 플래티넘으로!

![gold](/_img/2021-07-12/gold.jpg){: .align-center}
