---
title: "[Problems] 게리맨더링 (BAEKJOON: 17471)"
excerpt: "gerrymandering implementation by using combinations and bfs"
date: 2021-08-09
layout: single
classes: wide
category:
    - problems
tag:
    - python
    - baekjoon
    - dynamic programming
    - brute force
    - combinations
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-08-09/gerrymandering.jpg"
---

# 게리맨더링 (BAEKJOON: 17471)

**게리맨더링(gerrymandering)**이란 특정 후보자나 특정 정당에 유리하도록 선거구를 확정하는 것을 말합니다. 어원은 1812년 미국 메사추세츠주 주지사였던 엘브리지 게리로부터 유래되었습니다.

그는 자기 정당에 유리하도록 선거구를 분할하였는데, 그 모양이 마치 전설상의 괴물 **샐러맨더(Salamander)**와 비슷하여 **게리(Gerry)**의 이름을 합쳐 **게리맨더(Gerry-mander)**라고 부르게 되었습니다.

이후 이와 같이 선거구를 확정하는 것을 게리맨더링이라고 부르게 되었습니다.

## 문제 설명

[문제](https://www.acmicpc.net/problem/17471)에 등장하는 시장은 게리맨더링을 통해서 자신의 당에 유리하게 선거구를 확정했습니다. 견제할 권력이 없어진 시장이 권력을 부당하게 행사했기에, 이번에는 최대한 공평하게 선거구를 확정하려 합니다.

구간은 두 개로 나눠지며 나뉜 선거구는 아래 조건을 만족해야 합니다.

- 나눠진 선거구 그룹은 적어도 한 개의 선거구를 가지고 있어야 합니다
- 나눠진 그룹은 고립된 것이 있으면 안 됩니다

입력 조건은 다음과 같습니다.

- $2 \leq N \leq 10$
- $1 \leq$ 구역의 인구 수 $\leq 100$

## 예제 입력

```
6
5 2 3 4 1 2
2 2 4
4 1 3 6 5
2 4 2
2 1 3
1 2
1 2
```

## 예제 출력

```
1
```

## 문제 풀이

이 문제에서 얻을 수 있는 기술적 팁은 다음과 같습니다. 파이썬은 **asterisk**라는 가변인자 지정자를 이용할 수 있습니다. 이를 통해 고정되지 않은 인자들을 받을 수 있습니다.

```python
_, *adjacent = list(map(int, input().split()))
```

위의 코드는 **List** 의 첫 번째 원소는 **underbar(_)** 에 할당하고 나머지는 **adjacent** 라는 변수에 할당합니다.

풀이 알고리즘은 아래와 같습니다.

- `from itertools import combinations` 를 이용해 선거구 그룹 조합을 만듭니다
- 선거구 조합에 대한 `bfs()`를 동작시켜 그룹 조합에 포함된 선거구가 몇 개인지와 그 합을 반환합니다
- 두 선거구 조합에 `bfs()`를 진행하여 얻은 포함 선거구의 합이 $N$과 같다면 둘의 차를 확인하여 결과값을 **최신화**합니다

끝으로 이 문제에서 얻을 수 있는 직관은 다음과 같이 나타낼 수 있습니다.

- bfs 에서 append 하는 순간 chk 업데이트
- 그래프의 탐색은 bfs or dfs 두 가지 방법이 존재
- $N$ 개 중 $K$개 선택하는 문제는 ${}_N\mathrm{C}_K $ 고려


### 소스 코드

```python
from itertools import combinations
from collections import deque

# 노드 개수
N = int(input())

# 노드별 인구수 (index start with 1)
people = [0] + list(map(int, input().split()))

# 노드 별 인접 노드 저장
nodes = dict()
node_idx = 1
for i in range(N):
    _, *adjacent = list(map(int, input().split()))
    nodes[node_idx] = adjacent
    node_idx += 1

def bfs(node, region):
    # 인접노드
    node_queue = deque()

    # 탐색 시작 노드 삽입
    node_queue.append(node)

    # 방문한 노드 합계
    node_sum = 0
    visit_number = 0

    # 최초 방문한 노드 체크
    chk = [0 for _ in range(N+1)]
    chk[node] = 1
    node_sum += people[node]
    visit_number += 1

    # 인접 노드 탐색
    while bool(node_queue):
        current_node = node_queue.popleft()

        # 연결 된 노드
        adjacent_nodes = nodes.get(current_node)

        for n in adjacent_nodes:
            # 인접 노드가 region 에 들어있지 않은 경우 pass
            if not (n in region):
                continue
            # 인접 노드를 이미 방문한 경우
            if chk[n] == 1:
                continue

            # 방문할 노드 추가
            node_queue.append(n)
            chk[n] = 1
            node_sum += people[n]
            visit_number += 1

    return node_sum, visit_number

result = float('inf')

# 구역 나누기
for k in range(1, N//2+1):
    # 1 ~ N//2 까지
    for region1 in list(combinations(range(1, N+1), k)):
        region1 = list(region1)

        # region1 처음 노드로부터 탐색
        sum1, visit_num1 = bfs(region1[0], region1)

        # region2
        region2 = [i for i in range(1, N+1) if not (i in region1)]
        sum2, visit_num2 = bfs(region2[0], region2)

        if visit_num1 + visit_num2 == N:
            # print(f"region1: {region1}, visit: {visit_num1}, sum: {sum1}")
            # print(f"region2: {region2}, visit: {visit_num2}, sum: {sum2}")
            # print()
            result = min(result, abs(sum1 - sum2))

if result == float('inf'):
    print(-1)
else:
    print(result)
```