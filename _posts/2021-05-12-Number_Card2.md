---
title: "[Algorithm Problem] 숫자 카드2 (BAEKJOON: 10816번)"
excerpt: "이분탐색을 이용해 배열에 카드 개수를 확인하는 문제"
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

# Number Card 2 (숫자 카드2)

배열에 **Target**에 따른 **left most**와 **right most**의 위치를 알아보겠습니다.  
  
### 배열에 Target이 한 개일 때

![normal](/_img/2021-05-12/normal.jpg){: .align-center}

### 배열에 Target이 여러 개일 때

![normal](/_img/2021-05-12/multi_state.jpg){: .align-center}

### 배열에 Target이 없을 때

![normal](/_img/2021-05-12/none_target.jpg){: .align-center}

각각의 요소는 다음과 같이 구해질 수 있습니다. 따라서 먼저 **left most** 값을 구한 뒤, 이 값이 **MAX INDEX**를 넘거나 그 최소값이 같지 않다면 존재하지 않는 경우이므로 0을 출력하고 탐색을 멈춥니다. 이 과정을 통해 연산 과정을 줄일 수 있습니다.

그리고 **right most**를 구해 **Target**이 배열에 있음이 보장됐을 때, 그 개수를 인덱스를 이용해 구해냅니다.

```python
from math import floor

if __name__=="__main__":
    N = int(input())

    num_list = list(map(int, input().split(' ')))
    num_list.sort(key=lambda x: x)

    M = int(input())

    targets = list(map(int, input().split(' ')))

    for target in targets:
        left_point = 0
        right_point = len(num_list)

        # left most
        while left_point < right_point:
            middle_point = floor((left_point + right_point) / 2)

            if num_list[middle_point] < target:
                left_point = middle_point + 1
            else:
                right_point = middle_point

        # left most
        left_most = left_point

        # 처음 left most 를 이용해 있는지 확인
        if left_most == len(num_list) or num_list[left_most] != target:
            print(0, end=' ')
            continue

        left_point = 0
        right_point = len(num_list)

        # right most
        while left_point < right_point:
            middle_point = floor((left_point + right_point) / 2)

            if num_list[middle_point] <= target:
                left_point = middle_point + 1
            else:
                right_point = middle_point

        # right most
        right_most = right_point - 1

        # 개수 구하기
        print(right_most - left_most + 1, end=' ')
```