---
title: "[Algorithm Problem] 수 찾기 (BAEKJOON: 1920번)"
excerpt: "이분탐색을 이용해 배열에 수가 있는지 확인하는 문제"
date: 2021-05-12
layout: single
classes: wide
category:
    - algorithm problem
tag:
    - Python
    - Binary Search
    - Baekjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Find Number (수 찾기)

[Binary Search](https://wch18735.github.io/algorithm/Binary_Search/)에 관해서 자세히 배운 이론을 문제에 적용해 풀어보도록 하겠습니다. 이 문제를 두 가지 방법, **left most**와 **right most**를 이용해 해결하려 합니다. 가장 중요하게 기억해야 할 것은 아래와 같습니다.

한 가지 <span style="color:red; font-weight: bold;">추가</span> 할 것은 아래와 같습니다. 특정한 한 가지를 구하는 일반적인 것과 구분하기 위해 기억해두시면 좋겠습니다.

- $L=0$, $R=N$ 을 할당합니다.

그리고 나머지 조건들은 이전 포스트에서 나타냈던 꼭 기억해야 할 사항은 아래와 같습니다.

- while 조건을 $L < R$로 나타냅니다.
- **floor()** 를 통해 $L$과 $R$이 붙어 $R = L + 1$인 상태일 때 $m$ 이 $L$의 위치를 가리키도록 합니다.
- $L \larr m + 1$을 통해 종료를 보장합니다.
- 가장 간단한 형태를 가정합니다.
- 어떤 값을 Return할 것인지 결정합니다.

참고로 특정한 값 하나를 찾는 가장 간단한 방법은 아래 세 가지만 기억하면 됩니다.

- $L=0$, $R= N - 1$ 을 할당합니다.
- while 조건을 $L \le R$로 나타냅니다.
- 세 가지 if 문으로 분기하여 탐색을 실시합니다.

### Left Most 이용한 수 찾기

```python
from math import floor

if __name__=="__main__":
    N = int(input())

    # number list
    num_list = list(map(int, input().split(' ')))
    num_list.sort(key=lambda x: x)

    # 타겟 개수
    M = int(input())

    # 타겟 리스트
    target_list = list(map(int, input().split(' ')))

    # Left Most 구하는 방식
    for target_number in target_list:

        left_point = 0
        right_point = len(num_list)

        while left_point < right_point:
            # 항상 floor로 짝수일 때 왼쪽을 가리키게 함
            # L <- M + 1
            # R <- M
            middle_point = floor((left_point + right_point) / 2)

            # 가장 기초적인 상황에서 Left Most 이기 때문에 R이 움직이도록 설정
            if num_list[middle_point] < target_number:
                left_point = middle_point + 1
            else:
                right_point = middle_point


        if left_point == len(num_list):
            # 타깃 값이 매우 커서 없을 때
            print(0)
        else:
            if num_list[left_point] == target_number:
                print(1)
            else:
                print(0)
```

### Right Most 수 찾기

```python
from math import floor

if __name__=="__main__":
    N = int(input())

    # number list
    num_list = list(map(int, input().split(' ')))
    num_list.sort(key=lambda x: x)

    # 타겟 개수
    M = int(input())

    # 타겟 리스트
    target_list = list(map(int, input().split(' ')))

    # Right Most 구하는 방식
    for target_number in target_list:

        left_point = 0
        right_point = len(num_list)

        while left_point < right_point:
            # 항상 floor()로 짝수일 때 왼쪽을 가리키게 함
            # L <- M + 1
            # R <- M
            middle_point = floor((left_point + right_point) / 2)

            # 가장 단순한 상황에서 Right Most 이기 때문에 L이 움직이도록 설정
            if num_list[middle_point] <= target_number:
                left_point = middle_point + 1
            else:
                right_point = middle_point

        if num_list[right_point - 1] == target_number:
            print(1)
        else:
            print(0)
```

이 두 가지 코드와 일반적인 구동 코드를 이용해 다음과 같이 문제를 풀어낼 수 있습니다.

![find number correct](/_img/2021-05-12/find_number_correct.jpg){: .align-center}