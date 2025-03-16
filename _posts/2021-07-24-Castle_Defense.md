---
title: "[Problems] 캐슬 디펜스  (BAEKJOON: 17135번)"
excerpt: "Castle Defense"
date: 2021-07-24
layout: single
classes: wide
category:
    - problems
tag:
    - python
    - Beakjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 캐슬 디펜스 (BAEKJOON: 17135번)

이 문제는 삼성 A형에 출제 된 기출 문제입니다. 2시간 내에 푸는 것을 목표로 했습니다. 삼성SW역량테스트를 준비하기 위한 대표 문제로 아주 적절할 것 같습니다.

## 문제

캐슬 디펜스는 성을 향해 몰려오는 적을 잡는 턴 방식 게임입니다. 게임이 진행되는 곳은 크기가 $N \times M$인 격자판으로 나타낼 수 있습니다. 격자판은 $1 \times 1$ 크기 칸으로 나누어져 있고, 각 칸에 포함된 적의 수는 최대 하나입니다. 격자판의 $N$번 행의 바로 아래 $N+1$번 행의 모든 칸에는 성이 있습니다.

문제를 요약하면 아래와 같습니다.

- 성을 적에게 지키기 위해 배치할 궁수는 <span style="font-weight: bold; color: red">3명</span>입니다.
- 궁수는 성이 있는 칸에 배치할 수 있습니다.
- 한 칸에는 최대 1명의 궁수만 있을 수 있습니다.
- 각 턴마다 궁수는 적 하나를 공격할 수 있습니다.

아래는 문제 해결의 핵심인 공격 패턴입니다.

- 모든 궁수는 <span style="font-weight: bold; color:red;">동시에</span> 공격합니다.
- 궁수가 공격하는 적은 거리가 D 이하인 적 중 가장 가까운 적입니다.
- **가장 가까운 적이 여럿일 경우** 가장 왼쪽에 있는 적을 공격합니다.
- 같은 적이 여러 궁수에게 공격당할 수 있습니다.
- 공격받은 적은 게임에서 제외됩니다.

공격이 끝난 후 이동 패턴은 다음과 같습니다.

- 궁수의 공격이 끝나면 적은 한 칸 아래로 이동합니다.
- 성이 있는 칸으로 이동한 경우 게임에서 제외됩니다.
- 모든 적이 격자판에서 제외되면 게임이 끝납니다.

게임은 궁수의 위치를 잘 정해서 제거할 수 있는 적의 최대 수를 출력하는 것이 목표입니다.

격자판 $(r_1, c_1), (r_2, c_2)$ 의 거리는 $|r_1 - r_2| + |c_1 + c_2|$ 입니다.

## 입력

첫째 줄에 격자판 행의 수 N, 열의 수 M, 궁수의 공격 거리 제한 D가 주어집니다. 둘째 줄부터 N개의 줄에는격자판의 상태가 주어집니다. 0은 빈 칸, 1은 적이 있는 칸입니다.

## 예제 입력

```
5 5 1
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
1 1 1 1 1
```

## 예제 출력

```
3
```

## 해석

![castle](/_img/2021-07-24/castle1.jpg){: .align-center}

사거리가 1인 경우 가장 까가운 위쪽의 적에게 화살을 쏩니다. 그리고 남은 2명의 적이 아래로 내려오고 게임이 끝나게 됩니다.

저는 문제를 푸는데 핵심은 **동시 처리**라고 생각했습니다. **한 턴**에 동시에 화살에 맞는 적이 발생 가능한 조건이 문제의 난이도를 높였습니다.

한 턴은 <span style="color:#DDDD22; font-weight:bold;">궁수</span>들이 위치한 행을 for 문으로 도는 단위입니다. 해당 턴에서 화살에 맞은 적을 바로 **0**으로 바꾸는 것이 아니라 **k** 값으로 초기화 합니다. 

한 턴이 모두 끝났을 때, **killed** 값을 가진 적을 모두 찾아 **0**으로 바꿔주며 처리한 숫자에 더해줍니다.

이로써 궁수는 가장 가까운 거리에 있는 적을 찾을 때, **1**과 **k값**을 발견하면 화살을 쏘고 탐색을 마칩니다.

## 소스코드 해석

풀이에서는 기능별로 함수를 나눠 정리했습니다. 느긋하게 개발할 때는 최대한 영어를 쓰려 노력하지만 급한 경우는 한글로 정리하는 편입니다.

### check_diamond(y, x, D, enemy)

이 함수는 아래 세 가지를 인자로 받습다다. 

- 현재 궁수의 2차원 위치
- 궁수 사거리
- 맵 정보

세 정보를 바탕으로 현재 궁수 위치에서 가장 가까운 적을 찾아 처리합니다. 현재 궁수 위치에서 $1 \sim D$ 순서로 적을 탐색합니다. 탐색 모양이 반 다이아몬드 형태를 띄기 때문에 `check_diamond` 라는 이름을 붙였습니다.

```python
# point[y][x] 에 대하여 거리 D를 체크하는 함수
def check_diamond(y, x, D, enemy):
    global result

    # target list
    targets = []

    # 가장 가가운 거리
    for d in range(1, D + 1):
        # 가장 가까운 행
        for y_idx in range(y-1, y-d-1, -1):
            # 가까운 거리가 여러 개일 때 왼쪽부터 고려
            for x_idx in range(x-d, x+d+1):
                # x_idx 의 범위가 허용범위일 때
                if not (0 <= x_idx < M): continue
                # y_idx 의 범위가 허용범위일 때
                if not (0 <= y_idx < N): continue
                # 사정거리 안에 있을 때
                if not (abs(y_idx-y) + abs(x_idx-x) <= d): continue
                # 해당 target 위치가 1 or 'k' 일 때
                if not (enemy[y_idx][x_idx] == 1 or enemy[y_idx][x_idx] == 'k'): continue

                # 해당 타겟을 추가
                targets.append((y_idx, x_idx))

        # 거리마다 탐색하며 있는 경우
        if len(targets) == 0:
            continue
        else:
            targets.sort(key=lambda x:x[1])
            y_idx, x_idx = targets[0]
            if enemy[y_idx][x_idx] == 1:
                enemy[y_idx][x_idx] = 'k'
                result += 1
                return

            # 이미 한 번 쐈을 경우 kill 세지 않음
            elif enemy[y_idx][x_idx] == 'k':
                return
```

하나라도 적을 찾으면 해당 위치에 `K` 마크를 남깁니다. 그러나 해당 위치에 적이 있었으나 이미 처리된 경우, 죽인 수를 추가하지 않고 함수를 종료합니다.

가장 가까운 것이 여러 개가 존재하는 경우에는 먼저 후보들을 **List**에 저장합니다. `list.sort(key=lambda x:x[1])` 을 이용하면 **tuple**의 두 번째 인자를 기준으로 정렬할 수 있습니다.

따라서 한 행을 진행하며 궁수의 위치마다 해당 함수를 호출해 값을 업데이트 합니다.

### eliminate_kill(enemy)

이 함수는 맵 상에서 `K` 값으로 마킹된 위치를 모두 0으로 변환해주는 함수입니다. 이렇게 간단하지만 줄일 수 있는 코드들은 바깥으로 빼두는 것이 나중에 코드를 분석하는데 도움을 줍니다.

```python
def elimniate_kill(enmy):
    for y_idx in range(N):
        for x_idx in range(M):
            if enmy[y_idx][x_idx] == 'k':
                enmy[y_idx][x_idx] = 0
```

### dfs(cnt, sidx)

`comblist` 는 `combination list` 의 약자입니다. **dfs**를 통해서 주어진 **M열** 중 궁수가 설 수 있는 세 자리를 골라냅니다.

궁수가 설 수 있는 자리를 모두 고르면 `solution()` 함수를 호출해 처리하는 적의 수를 구합니다.

```python
def dfs(cnt, sidx):
    if cnt == 3:
        solution()
        return

    if sidx == M:
        return

    # dfs
    comblist[sidx] = 1
    dfs(cnt + 1, sidx + 1)
    comblist[sidx] = 0
    dfs(cnt, sidx + 1)
```

여기서 주의 깊게 볼 것은 **dfs의 형태**입니다.

```python
값 할당
DFS() 호출
값 초기화
```

위의 형태는 DFS 함수 인자로 데이터를 넘겨 **Out of memory**가 발생할 때, 시도해볼 수 있는 방법입니다. 실제로 저는 여러 코딩테스트에서 이 방법으로 **OOM 문제**를 해결했습니다.

### solution()

솔루션 함수가 호출될 때마다 정답인 **answer**가 구해집니다. **result** 궁수의 위치에 따라 처리된 수 입니다. 결과 값을 이전에 구해진 정답과 비교해 최대 값을 저장합니다.

```python
# 주어진 comblist 를 이용해 총 해치우는 적 수 계산
def solution():
    copied_castle = deepcopy(castle)

    # 아래서부터 순회하며 결과 값 해석
    for y_idx in range(N, 0, -1):
        for x_idx, archer in enumerate(comblist):
            if archer == 1:
                # 궁수가 배치된 곳일 때
                check_diamond(y_idx, x_idx, D, copied_castle)

        # turn이 끝날 때마다 kill 된 적들 0으로 변환
        elimniate_kill(copied_castle)

    global answer
    global result
    answer = max(answer, result)
    result = 0
```

## 문제에서 중요하게 다뤄야하는 점

이 문제에서는 아래와 같이 코딩 테스트에 활용할 수 있는 스킬을 배울 수 있다고 생각합니다. 

1. Diamond 범위 탐색
2. 탐색 순서를 for문 순서를 이용해 조절할 수 있다는 것
3. 가장 왼쪽, 오른쪽 등은 list.sort(key=lambda x:(x[0], x[1])) 을 이용해 정렬할 수 있다는 것
4. global 을 이용해 함수 내부에서도 전역변수 접근하는 방법
5. combination -> dfs() 를 통해 구현하는 방법
6. deepcopy 를 하지 않으면 reference 가 전달되어 함수 내부에서 외부 값이 변경될 수 있다는 것

## 소스 코드

```python
from copy import deepcopy

# 세로, 가로, 사정거리
N, M, D = map(int, input().split(' '))

# 궁수 위치는 시작 때 고정
# DP + DFS
# 각 턴마다 죽인 개수 합 저장
castle = [list(map(int, input().split(' '))) for i in range(N)]
result = 0
answer = 0

# point[y][x] 에 대하여 거리 D를 체크하는 함수
def check_diamond(y, x, D, enemy):
    global result

    # target list
    targets = []

    # 가장 가가운 거리
    for d in range(1, D + 1):
        # 가장 가까운 행
        for y_idx in range(y-1, y-d-1, -1):
            # 가까운 거리가 여러 개일 때 왼쪽부터 고려
            for x_idx in range(x-d, x+d+1):
                # x_idx 의 범위가 허용범위일 때
                if not (0 <= x_idx < M): continue
                # y_idx 의 범위가 허용범위일 때
                if not (0 <= y_idx < N): continue
                # 사정거리 안에 있을 때
                if not (abs(y_idx-y) + abs(x_idx-x) <= d): continue
                # 해당 target 위치가 1 or 'k' 일 때
                if not (enemy[y_idx][x_idx] == 1 or enemy[y_idx][x_idx] == 'k'): continue

                # 해당 타겟을 추가
                targets.append((y_idx, x_idx))

        # 거리마다 탐색하며 있는 경우
        if len(targets) == 0:
            continue
        else:
            targets.sort(key=lambda x:x[1])
            y_idx, x_idx = targets[0]
            if enemy[y_idx][x_idx] == 1:
                enemy[y_idx][x_idx] = 'k'
                result += 1
                return

            # 이미 한 번 쐈을 경우 kill 세지 않음
            elif enemy[y_idx][x_idx] == 'k':
                return

# 길이 M, 1의 개수 3개인 리스트 반환하는 함수
comblist = [0 for _ in range(M)]

# 이중 리스트 프린트
def print_list(enemy):
    for i in range(N):
        for j in range(M):
            print(enemy[i][j], end=' ')
        print()
    print()

def elimniate_kill(enmy):
    for y_idx in range(N):
        for x_idx in range(M):
            if enmy[y_idx][x_idx] == 'k':
                enmy[y_idx][x_idx] = 0

# 주어진 comblist 를 이용해 총 해치우는 적 수 계산
def solution():
    copied_castle = deepcopy(castle)

    # 아래서부터 순회하며 결과 값 해석
    for y_idx in range(N, 0, -1):
        for x_idx, archer in enumerate(comblist):
            if archer == 1:
                # 궁수가 배치된 곳일 때
                check_diamond(y_idx, x_idx, D, copied_castle)

        # turn이 끝날 때마다 kill 된 적들 0으로 변환
        elimniate_kill(copied_castle)

    global answer
    global result
    answer = max(answer, result)
    result = 0

def dfs(cnt, sidx):
    if cnt == 3:
        solution()
        return

    if sidx == M:
        return

    # dfs
    comblist[sidx] = 1
    dfs(cnt + 1, sidx + 1)
    comblist[sidx] = 0
    dfs(cnt, sidx + 1)

def main():
    dfs(0, 0)
    print(answer)

if __name__=="__main__":
    main()
```