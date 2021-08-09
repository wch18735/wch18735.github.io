---
title: "[Coding Test] 파이프 옮기기1  (BAEKJOON: 17070)"
excerpt: "Moving pipe"
date: 2021-07-26
layout: single
classes: wide
category:
    - coding test
tag:
    - python
    - Beakjoon
    - dynamic programming 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-07-26/pipe.jpg"
---

# 파이프 옮기기 (BAEKJOON: 17070번)

파이프 옮기기 시리즈 첫 번째입니다. **Dynamic Programming** 을 이용해 푸는 문제입니다. [이곳](https://www.acmicpc.net/problem/17070)에서 풀어보실 수 있습니다.

## 문제

파이프를 밀어서 가장 끝에 도달시킬 수 있는 경우의 수를 찾아야 합니다. 문제 포인트는 아래와 같이 요약할 수 있습니다.

- 새 집의 크기는 $N \times N$ 격자판으로 나타낼 수 있습니다.
- 칸은 $1 \times 1$ 크기입니다.
- 각 칸은 행번호 $r$ 과 열번호 $c$ 정보로 $(r, c)$ 로 나타낼 수 있습니다.
- 행과 열 번호는 $1$ 부터 시작합니다.
- 파이프는 2개의 컨을 연속으로 차지합니다.

다음은 파이프 배치 룰입니다.

- 파이프는 3가지 방향의 배치가 가능합니다.
![pipe](/_img/2021-07-26/pipe.jpg)
- 파이프를 밀면서 이동할 때, 벽을 긁으면 안 됩니다.
- 파이프를 밀 수 있는 방향은 →, ↘, ↓ 세 가지 입니다.
- 미는 방향은 오른쪽, 아래, 또는 오른쪽 아래 대각선 방향이어야만 합니다.

정리하자면 각 배치마다 가능한 미는 방향은 다음과 같습니다.

- 가로는 2가지 방향으로 밀 수 있습니다.
- 세로는 2가지 방향으로 밀 수 있습니다.
- 대각선은 3가지 방향으로 밀 수 있습니다.

## 예제 입력

```
5
0 0 1 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```

## 예제 답

```
0
```

## 풀이

위의 예시는 **파이프가 배치될 때 벽을 긁으면 안 된다** 룰에 따라 하나도 도달할 수 없음을 보여줍니다. 문제 풀이에 헷갈리지 않으라는 친절한 예시였습니다.

저는 각 상태에 따른 조건을 정의하여 **BFS**를 이용했습니다. 문제에서 주어진 예시들은 모두 통과했으나 실제 제출에서는 TimeOut을 받았습니다.

아래는 Time Out 풀이입니다.

```python
from collections import deque
import copy

# 길이
N = int(input())

# 벽 입력
bricks = [list((map(int, input().split(' ')))) for i in range(N)]

# 지나간 자리 DP
chk = [[[] for i in range(N)] for j in range(N)]

# 상태: 방향 start_(dy, dx), end_(dy, dx)
# 벡터에 따른 상태 판단 (가로, 세로, 대각)
# end(y2, x2) - start(y1, x1)
direction_dict = {
    (0, 1): [((0, 1), (0, 1)), ((0, 1), (1, 1))], # 오른쪽 1칸, 오른쪽 대각 1칸
    (1, 0): [((1, 0), (1, 0)), ((1, 0), (1, 1))], # 아래 1칸, 아래 대각 1칸
    (1, 1): [((1, 1), (0, 1)), ((1, 1), (1, 0)), ((1, 1), (1, 1))], # 가로로 1칸, 세로로 1칸, 대각으로 1칸
}

state_dict = {
    (0, 1): "horizon",
    (1, 0): "vertical",
    (1, 1): "slide"
}

def copypipe(pipe):
    return copy.deepcopy(pipe)

def calculvec(cstate):
    startp = cstate[0]  # (y1, x1)
    endp = cstate[1]    # (y2, x2)
    return endp[0] - startp[0], endp[1] - startp[1]

def return_state(state):
    return state_dict.get(state)

def bfs():
    # (1, 1)에서 출발하여 (N, N)에 도달하는 경우의 수
    # -> (N-1,N-1) 이면 도착!!

    # 파이프 객체 (시작 위치, 끝 위치) -> (y, x)
    pipe = [[0, 0], [0, 1]]

    # 파이프 상태 저장
    pipe_positions = deque()
    pipe_positions.append(copypipe(pipe))

    answer = 0

    # 탐색
    while bool(pipe_positions):
        # 현재 파이프 상태
        current_pipe = pipe_positions.popleft()
        state = calculvec(current_pipe)

        # 현재 파이프 도착 확인
        if current_pipe[1][0] == N-1 and current_pipe[1][1] == N-1:
            answer += 1
            continue

        # 상태에 따른 방향
        directions = direction_dict.get(state)

        # 예외처리
        if directions == None: break

        # 탐색
        for dd in directions:
            # start dy and dx, end dy and dx
            dd_s, dd_e = dd

            # 현재 상태 파이프 복사
            tmp_pipe = copypipe(current_pipe)

            # 파이프 이동
            tmp_pipe[0][0] = tmp_pipe[0][0] + dd_s[0]   # 시작 y
            tmp_pipe[0][1] = tmp_pipe[0][1] + dd_s[1]   # 시작 x
            tmp_pipe[1][0] = tmp_pipe[1][0] + dd_e[0]   # 끝 y
            tmp_pipe[1][1] = tmp_pipe[1][1] + dd_e[1]   # 끝 x

            # 끝 부분이 범위를 넘어간 경우 continue
            if not (tmp_pipe[1][0] < N): continue
            if not (tmp_pipe[1][1] < N): continue
            # 벽을 만난 경우
            if bricks[tmp_pipe[1][0]][tmp_pipe[1][1]] == 1: continue
            # 대각선일 때, 겹치는 경우
            if return_state(calculvec(tmp_pipe)) == "slide":
                s_x = tmp_pipe[0][1]
                s_y = tmp_pipe[0][0]
                if bricks[s_y + 1][s_x] == 1 or bricks[s_y][s_x + 1] == 1:
                    continue

            # 상태에 추가
            pipe_positions.append(copypipe(tmp_pipe))

    return answer

def main():
    print(bfs())
    # print(chk)

if __name__=="__main__":
    main()
```

## 정답 소스코드

아래는 정답 소스코드입니다. 풀이 방법은 정말 간단했습니다. 특정한 좌표 $(x, y)$ 에 도달했을 때 아래 과정을 거칩니다.

- CACHE[x][y] 는 위치 $(x, y)$는 [a, b, c] 값을 가집니다.
- a, b, c 는 각각 가로, 세로, 대각선으로 도달할 수 있는 경우의 수를 뜻합니다.
- $(x, y)$ 좌표의 왼쪽 대각선, 위, 왼쪽 좌표 값을 구합니다.
- 목적 좌표에 도달할 때 가로, 세로, 대각선이 될 수 있는 경우를 상태 저장합니다.

간단하게 예시를 들어보겠습니다. 목적 좌표로 **가로로 도달할 수 있는 상황**은 왼쪽 옆 가로와 왼쪽 옆 대각입니다. 따라서 왼쪽 좌표의 a 값, 왼쪽 대각선 좌표의 b 값을 더해 현재 위치의 a 에 더해줍니다.

이렇게 for 문을 돌며 값을 채워준 뒤 마지막 CACHE[x][y] 의 원소 값인 `a + b + c`를 해주면 답이 됩니다.

```python
# input
N = int(input())

# bricks
bricks = [[0 for i in range(N+1)]] + [[0] + list(map(int, input().split(' '))) for i in range(N)]

# DP 풀이 (Memoization or Terbulance)
# 도착했을 때 가로, 세로, 대각인 상태 DP를 만들 수 있다
# (N+1) * (N+1) * 3 형태 DP
dp = [[[0, 0, 0] for i in range(N+1)] for j in range(N+1)]
# (1, 2)의 가로는 1 가능
dp[1][2][0] = 1

def print_dp():
    for i in range(1, N+1):
        for j in range(1, N+1):
            print(sum(dp[i][j]), end=' ')
        print()

def main():
    for y in range(1, N + 1):
        for x in range(1, N + 1):
            if bricks[y][x] == 1:
                continue

            # 도착했을 때 가로 (0)
            # 왼 쪽 옆 가로
            # 왼 쪽 옆 대각
            dp[y][x][0] += (dp[y][x - 1][0] + dp[y][x - 1][2])

            # 도착했을 때 세로 (1)
            # 위 세로
            # 위 대각
            dp[y][x][1] += (dp[y - 1][x][1] + dp[y - 1][x][2])

            # 도착했을 때 대각 (2, 위 옆으로 벽이 없을 때)
            # 대각선 세로
            # 대각선 가로
            # 대각선 대각
            if bricks[y - 1][x] == 1 or bricks[y][x - 1] == 1:
                continue
            dp[y][x][2] += (dp[y - 1][x - 1][0] + dp[y - 1][x - 1][1] + dp[y - 1][x - 1][2])

    print(sum(dp[N][N]))

if __name__=="__main__":
    main()
```