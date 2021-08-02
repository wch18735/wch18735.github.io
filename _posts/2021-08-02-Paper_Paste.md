---
title: "[Coding Test] 색종이 붙이기 (BAEKJOON: 17136)"
excerpt: "paper paste by dfs"
date: 2021-08-02
layout: single
classes: wide
category:
    - coding test
tag:
    - python
    - baekjoon
    - dynamic programming
    - dfs 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 색종이 붙이기 (BAEKJOON: 17136)

가지고 있는 색종이들을 효율적으로 사용해 모든 타일을 채우는 경우 중 가장 작은 색종이 수를 구하는 문제입니다.

타일을 붙이고, 떼는 과정을 하나의 DFS 턴으로 정하면 문제를 쉽게 풀 수 있습니다. 현재 남아있는 종이 수, 종이가 붙어있는 판 등을 전역변수로 설정해줍니다.

**DFS**로 문제해결 방법을 정했다면 아래 두 가지를 생각해 알고리즘을 짜봅니다.

- 상태를 공유하는 전역변수를 설정해 메모리 효율을 추구하는 것
- 분기되며 퍼지지만 한 줄기를 모두 탐색하는 과정을 거치고, 다시 전환되는 과정에서 이전 상태를 회복한다는 것

## 느낀점

이 문제를 풀며 다음을 생각해 볼 수 있었습니다.


- **dfs** 단위는 방향, Turn 등 다양할 수 있다
- Pruning 은 `dfs(variable)` 의 variable 에 따라 더 이상 탐색하지 않아도 되는 부분을 잘라내 속도를 향상
- 논리적 과정을 직관적으로 옮길 때 **dfs()**를 사용하면 좋음
- Greedy 함이 항상 옳은 것은 아님!!

## 문제 해결 알고리즘

먼저 다음과 같은 기능들을 구현합니다.

- `coverable(y, x, length)`: $(x, y)$ 위치를 좌측 상단으로 길이 **length** 인 종이로 덮을 수 있는가
- `cover_map(y, x, length)`:  $(x, y)$ 위치를 좌측 상단으로 길이 **length** 인 종이로 덮는다
- `leap_map(y, x, length)`: $(x, y)$ 위치를 좌측 상단으로 붙어있는 종이를 떼어낸다

이렇게 구현한 함수들을 논리적 과정대로 구현합니다. 먼저 **붙여보고**, **다음 붙일 곳을 찾고**, **떼어내고**, **다른 붙일 수 있는 것을 찾는** 과정을 DFS로 구현합니다.

문제 푸는 과정에서 그래프를 그려보면 코드를 더 쉽게 떠올릴 수 있습니다.

## 소스코드

```python
# 종이
MAP = [list(map(int, input().split(' '))) for i in range(10)]

# 정답
answer = -1

# 남은 종이
papers = [0, 5, 5, 5, 5, 5]

def get_start_point():
    for y in range(10):
        for x in range(10):
            if MAP[y][x] == 1:
                return y, x
    return -1, -1

def coverable(y, x, length):
    # 거리 확인
    if not (y + length - 1 < 10):
        return False
    if not (x + length - 1 < 10):
        return False
    # 사용할 수 있는 개수 남았는지 확인
    if papers[length] == 0:
        return False

    # 사각형 종이를 덮을 수 있는지 확인
    for dy in range(length):
        for dx in range(length):
            if MAP[y + dy][x + dx] == 0:
                return False
    return True

def cover_map(y, x, length):
    for dy in range(length):
        for dx in range(length):
            MAP[y + dy][x + dx] = 0

def leap_map(y, x, length):
    for dy in range(length):
        for dx in range(length):
            MAP[y + dy][x + dx] = 1

def print_map():
    for i in range(10):
        for j in range(10):
            print(MAP[i][j], end=' ')
        print()
    print()

def dfs(paper_num):
    global answer
    # dfs가 종료되는 경우
    # 1. 모든 종이가 채워졌을 때
    # 2. 덮을 수 있는 종이가 더 이상 존재하지 않을 때
    # 3. 더 많은 경우를 탐색하고 있을 때

    if answer != -1 and paper_num >= answer:
        return

    # 모든 종이가 채워졌는지 확인
    y, x = get_start_point()
    if y == -1 and x == -1:
        # 모든 종이가 채워졌을 때, answer == -1 이면 바로 저장
        if answer == -1:
            answer = paper_num
        # 최소 값이면 answer 저장
        elif answer != 0 and paper_num < answer:
            answer = paper_num
        return

    # 덮을 수 있는 종이가 존재하는지 확인
    for i in range(1, 6):
        if coverable(y, x, i):
            # 해당 길이의 종이 사용
            papers[i] -= 1
            # 덮은 부분은 0으로 만듬
            cover_map(y, x, i)
            # 다음 1로 시작하는 부분 탐색
            dfs(paper_num + 1)
            # 사용한 길이 종이를 떼어 냄
            papers[i] += 1
            # 덮은 종이를 다시 떼어냄
            leap_map(y, x, i)


def main():
    dfs(0)
    print(answer)
    pass

if __name__=="__main__":
    main()
```