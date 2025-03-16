---
title: "[Problems] 배열 돌리기 1 (BAEKJOON: 17070)"
excerpt: "rotate array 1"
date: 2021-08-10
layout: single
classes: wide
category:
    - problems
tag:
    - python
    - Beakjoon
    - deque 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-08-10/rotate_concept.jpg"
---

# 배열 돌리기 1 (BAEKJOON: 16926)

[이번 문제](https://www.acmicpc.net/problem/16926)는 주어진 배열을 다음과 같이 돌리는 문제입니다. 저는 가장 바깥 쪽부터 **한 층씩 공략한다**는 개념으로 접근했습니다.

![rotate concept](/_img/2021-08-10/rotate_concept.jpg){: .align-center}

주어지는 회전 수에 따라 칸을 옮기는 방법으로 해결했습니다. 제 풀이의 핵심은 한 줄씩 담는 과정입니다. 이 때, 값들과 함께 좌표를 함께 저장했습니다.

## 입력

- 첫째 줄에 배열의 크기 $N, M$과 회전 수 $R$이 주어집니다.
- 둘째 줄부터 $N$개의 줄에 배열 $A$의 원소 $A_{ij}$가 주어집니다.

## 제한

- $2 \leq N, M \leq 300$
- $1 \leq R \leq 1,000$
- $min(N, M)$  $mod$ $2 = 0$
- $1 \leq A_{ij} \leq 10^{8}$

## 예제 입력

```
4 4 2
1 2 3 4
5 6 7 8
9 10 11 12
13 14 15 16
```

## 예제 출력

```
3 4 8 12
2 11 10 16
1 7 6 15
5 9 13 14
```

## 문제 풀이

이 문제에서 배워야 할 것은 <span style="color: red; font-weight: bold">자기만의 달팽이 그리는 방법</span>이라 생각합니다. 이 배열 돌리기 시리지를 잘 풀어보시면 코딩 테스트에서 유용하게 쓸 수 있는 팁들을 익히실 수 있을 것이라 생각합니다. 

저는 먼저 다음과 같이 입력을 받았습니다.

```python
# 행, 열, 회전 횟수
N, M, R = map(int, input().split(' '))

# 입력
MAP = [list(map(int, input().split(' '))) for _ in range(N)]
CHK = [[0 for i in range(M)] for j in range(N)]
```

그리고 방향을 시계방향으로 `direction`이라는 변수에 저장했습니다. 이는 방향을 전환할 때, 달팽이 모양을 그리는 방법으로 사용됩니다.

```python
# direction (우, 하, 좌, 상 => 시계방향)
direction = [(0, 1), (1, 0), (0, -1), (-1, 0)]
```

그리고 중요한 작업 중 하나가 사각형을 감싸는 선 개수를 체크하는 것입니다. 개수는 **가로와 세로 길이 중 짧은 것의 반**으로 구할 수 있습니다. 이는 조건에도 명시되어 있습니다.

```python
    # 사각형을 감싸는 선 갯수
    que_num = min(N//2, M//2)

    # 선에 포함되는 큐
    que_list = [deque() for i in range(que_num)]
    value_list = [deque() for i in range(que_num)]
```

그렇게 구한 총 라인의 개수만큼 선을 만듭니다. `que_list`에는 좌표, `value_list`에는 좌표 값에 해당하는 값이 **순서대로** 저장됩니다. 이때, 순서는 시계방향으로 도는 달팽이 모양입니다.

이후에는 $(0, 0)$ 에서부터 탐색을 시작합니다. 탐색 규칙은 아래와 같습니다.

- 현재 선에 아무것도 들어있지 않다면 현재 좌표와 좌표에 해당하는 값을 추가합니다
- `direction` 을 기준으로 다음 좌표를 계산합니다
- 방문한 곳은 `CHK[y][x]` 배열에 표시를 남깁니다
- 범위를 벗어난 곳에서는 방향을 회전합니다
    - 현재 방향이 위가 아니라면 방향을 전환합니다
    - 현재 방향이 위인데 방향을 전환해야하는 상황이라면 선 안쪽으로 이동합니다
    - 모든 선에 요소를 다 채웠다면 빠져 나옵니다

이렇게 탐색을 했다면 각 줄마다 회전 수만큼 `rotate`를 시킵니다. 저는 아래와 같이 구현했습니다.

```python
# 회전
for l in value_list:
    for _ in range(R):
        l.append(l.popleft())
```

위치 좌표들을 순서대로 가지고 있는 `queue` 와 `zip` 연산을 통해 매칭된 값들을 가져와 배열에 저장합니다. 이 배열을 출력해주면 됩니다.

## 소스코드

```python
from collections import deque

# 행, 열, 회전 횟수
N, M, R = map(int, input().split(' '))

# 입력
MAP = [list(map(int, input().split(' '))) for _ in range(N)]
CHK = [[0 for i in range(M)] for j in range(N)]

def print_map(blocks = None):
    if blocks == None:
        for y in range(N):
            for x in range(M):
                print(MAP[y][x], end=' ')
            print()
        print()
    else:
        for y in range(N):
            for x in range(M):
                print(blocks[y][x], end=' ')
            print()
        print()

# direction (우, 하, 좌, 상 => 시계방향)
direction = [(0, 1), (1, 0), (0, -1), (-1, 0)]


def main():
    # rotate
    # 사각형을 감싸는 선 갯수
    que_num = min(N//2, M//2)

    # 선에 포함되는 큐
    que_list = [deque() for i in range(que_num)]
    value_list = [deque() for i in range(que_num)]

    # 큐에 담기
    start_x, start_y = 0, 0
    dir_idx = 0
    que_idx = 0

    while True:
        value = MAP[start_y][start_x]

        # Initial condition
        if len(que_list[que_idx]) == 0:
            que_list[que_idx].append((start_y, start_x))
            value_list[que_idx].append(value)
            CHK[start_y][start_x] = 1
        else:
            # 마지막 위치와 현재 위치 비교
            last_point = que_list[que_idx].pop()

            # 현재 위치가 이전 위치와 다를 때
            if not last_point == (start_y, start_x):
                # 이전 위치 다시 복구
                que_list[que_idx].append(last_point)
                value_list[que_idx].append(value)

            # 현재 위치 넣기
            que_list[que_idx].append((start_y, start_x))
            # 현재 위치 체크
            CHK[start_y][start_x] = 1

        # 다음 방향 진행 벡터
        dy, dx = direction[dir_idx]

        # 다음 진행할 x + dx 가 유효한 범위인지 확인
        if not (0 <= start_x + dx < M):
            dir_idx = (dir_idx + 1) % 4
            continue
        # 다음 진행할 y + dy 가 유효한 범위인지 확인
        if not (0 <= start_y + dy < N):
            dir_idx = (dir_idx + 1) % 4
            continue
        # 다음 방향이 기존에 밟았던 땅일 때
        if CHK[start_y + dy][start_x + dx] == 1:
            # 현재 방향이 위라면 다음 큐 리스트로 이동
            if dir_idx == 3:
                start_x += 1
                que_idx += 1
                if que_idx == que_num:
                    break
            # 방향만 전환
            dir_idx = (dir_idx + 1) % 4
            continue

        # 모든 조건에 부합하지 않을 때
        start_x += dx
        start_y += dy

    # 회전
    for l in value_list:
        for _ in range(R):
            l.append(l.popleft())

    # 답 프린트
    for que_idx, l in enumerate(que_list):
        for value, point in zip(value_list[que_idx], l):
            y, x = point
            MAP[y][x] = value

    print_map()

if __name__=="__main__":
    main()
```
