---
title: "[Algorithms] Meet in the middle"
excerpt: "Search technique which is used when the input is small but not as small that brute force can be used"
date: 2021-05-12
layout: single
classes: wide
category:
    - algorithm
tag:
    - algorithms
    - Meet in the middle
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Meet in the middle (가운데서 만나기)

**Meet in the middle** 은 Brute force를 이용할 때 조금은 부담되는 경우 사용하는 알고리즘입니다. 특히 부분집합을 구하는 경우 원소의 개수 N에 따라 연산은 $2^{N}$이 됩니다. 만약 N이 40이라면 이는 대략 $10^{12}$가 되어 모든 경우를 조사하는데 많은 시간을 쏟게 됩니다.  
  
**Meet in the middle** 은 **divide and conquer**과 비슷하게 아래와 같은 과정을 거칩니다. 
- 집합을 두 부분으로 나눕니다. 
- 나뉜 집합에 필요한 연산을 각각 수행합니다.
- 두 결과를 이용해 원하는 조건을 만족하는 경우의 수를 구합니다.  
  
**divide and conquer** 알고리즘과는 달리 이는 나뉘어진 문제를 재귀적으로 나누어 Atomic한 문제를 해결하는 방식이 아니라는 것입니다. 그치만 이 과정을 통해 어떠한 연산상의 이득을 얻을 수 있는지 예제를 통해 확인해보겠습니다.  
  
[1450: 냅색문제](https://www.acmicpc.net/problem/1450)는 Dynamic Programming의 대표적인 예제이기도 합니다. 이를 **Meet in the middle**을 이용한 **Brute force**로 풀어보겠습니다. 문제는 다음과 같습니다.

- 주어지는 물건의 개수 N $(1 \le N \le 30)$
- 최대 넣을 수 있는 무게 C $(0 \le C \le 10^{9})$
- 물건의 무게 X $(0 \le X \le 10^{9})$

이렇게 주어지는 물건들에 대해서 N개 물건 중 몇 개를 골라 가방에 넣는 방법의 수를 구하는 프로그램을 작성하는 것입니다. 예제 입력으로부터 나타나는 예제출력을 통해 아무것도 넣지 않는 것도 경우의 수에 포함된다는 것을 알 수 있습니다. 여기에서 저는 **공집합**을 생각했습니다. 이 문제는 **주어진 집합의 부분집합의 합 값**이 **C보다 작은 것의 개수**를 구하는 것과 같습니다.  
  
모든 부분집합의 개수를 구하는 경우의 수는 최대 $2^{30}$으로 이를 모두 수행하려면 제한시간 1초를 만족하지 못하고 시간초과가 발생할 수 있습니다. 이 문제에 앞에서 설명한 **Meet in the middle**을 적용해보겠습니다.

#### 1. 집합을 두 부분으로 나누기

먼저 집합을 두 부분으로 나누겠습니다. 입력으로 받은 **weights**를 **list_A**와 **list_B**로 나누어 담겠습니다. 이때, **ceil**을 통해 현재값과 같거나 큰 최소의 정수로 **N/2** 인덱스를 설정합니다. 만약 원소가 하나가 주어질 때, 한 개의 원소를 **list_A**에 넣기 위함입니다.

```python
    N, C = map(int, input().split(' '))

    weights = list(map(int, input().split(' ')))

    list_A = weights[:ceil(N/2)]
    list_B = weights[ceil(N/2):]

    # 리스트 길이 (원소 개수)
    len_A = len(list_A)
    len_B = len(list_B)

    # 부분 집합 합
    subset_a = []
    subset_b = []
```

#### 2. 각각에 연산을 진행

부분집합의 원소 합 값을 이용해 문제를 풀 것이기 때문에 두 개로 나뉘어진 배열의 모든 부분집합의 원소 합 값을 구해 배열에 저장합니다. 이때, **combinations**를 사용해 최적화된 연산을 수행합니다.  
  
**list_A**의 부분집합 합 값을 오름차순으로 정렬하는 이유는 검색시 이분탐색을 위함입니다. 이로써 모든 경우를 탐색하지 않고 찾아진 index의 왼쪽에 존재하는 부분집합의 합 값들은 조건을 만족함을 보장할 수 있습니다.
  
```python
    for num in range(len_A + 1):
        for sub_set in combinations(list_A, num):
            subset_a.append(sum(sub_set))

    for num in range(len_B + 1):
        for sub_set in combinations(list_B, num):
            subset_b.append(sum(sub_set))

    # list_A 오름차순 정렬
    subset_a.sort(key=lambda x: x)
```

#### 3. 이분탐색을 통한 조건에 맞는 경우 검색

이분탐색은 배열이 정렬되어있을 때, $O(log_2{N^{\prime}})$으로 검색 시간을 줄여줄 수 있습니다. **list_B**의 부분집합의 합 집합인 **subset_b**의 원소들을 하나씩 뽑아 이 원소와 합했을 때 **C**보다 작은 값을 보장하는 경계점을 조사합니다.  
  
```python
    # 답
    answer = 0

    # 정렬된 요소에 이분탐색 시행
    for element_b in subset_b:
        # 용량보다 큰 경우
        if element_b > C:
            continue

        # 이분 탐색
        left_idx = 0
        right_idx = len(subset_a)

        # 값을 만족하는 rightmost
        while left_idx < right_idx:
            mid = floor((left_idx + right_idx) / 2)
            if element_b + subset_a[mid] > C:
                right_idx = mid
            else:
                left_idx = mid + 1

        # right_idx -> 개수
        answer += right_idx
    
    print(answer)
```

### 소스코드

위를 간단하게 정리하면서 연산량을 정리해보겠습니다. 주어진 N개의 물건에 대하여

- A, B는 각각 $N / 2$ 정수를 가져갑니다. N이 홀수일 때는 A가 한 개의 원소를 더 가져갑니다.
- X, Y는 A와 B의 부분집합의 합 값들의 집합입니다. 이들은 각각 $2^{N/2}$의 연산을 통해 만들어질 수 있으며, 그 개수 역시 $2^{N/2}$입니다.
- 이 두 집합을 이용해 만들어진 배열을 합쳐 문제를 해결합니다.
    - 만약 그대로 brute force를 진행하면 이는 결국 $O((2^{N/2})^2)$ = $O(2^{N})$ 이 되어 연산에 효율이 사라집니다.
    - X를 오름차순으로 정렬하여 이분탐색을 이용하는 경우, 먼저 X를 정렬하는데 $O( log(2^{N/2}) )$의 연산을 필요로 합니다. 
    - $2^{N/2}$개의 부분집합의 합에 대하여 각각이 정렬된 X에 대한 이분탐색을 진행하므로 그 연산은 $O(2^{N/2}log(2^{N/2}))$ 으로 나타낼 수 있습니다.
    - 정렬에 필요한 연산량과 이분탐색에 필요한 연산을 합치면 이는 $O(2^{N/2}n)$으로 나타낼 수 있음을 알 수 있습니다. 

```python
from itertools import combinations
from math import floor, ceil

if __name__=="__main__":
    N, C = map(int, input().split(' '))

    weights = list(map(int, input().split(' ')))

    list_A = weights[:ceil(N/2)]
    list_B = weights[ceil(N/2):]

    # 리스트 길이 (원소 개수)
    len_A = len(list_A)
    len_B = len(list_B)

    # 부분 집합 합
    subset_a = []
    subset_b = []

    for num in range(len_A + 1):
        for sub_set in combinations(list_A, num):
            subset_a.append(sum(sub_set))

    for num in range(len_B + 1):
        for sub_set in combinations(list_B, num):
            subset_b.append(sum(sub_set))

    # list_A 오름차순 정렬
    subset_a.sort(key=lambda x: x)

    # 답
    answer = 0

    # 정렬된 요소에 이분탐색 시행
    for element_b in subset_b:
        # 용량보다 큰 경우
        if element_b > C:
            continue

        # 이분 탐색
        left_idx = 0
        right_idx = len(subset_a)

        # 값을 만족하는 rightmost
        while left_idx < right_idx:
            mid = floor((left_idx + right_idx) / 2)
            if element_b + subset_a[mid] > C:
                right_idx = mid
            else:
                left_idx = mid + 1

        # right_idx -> 개수
        answer += right_idx


    # 총 경우의 수
    print(answer)
```

## References
- [GeeksforGeeks: Meet in the middle](https://www.geeksforgeeks.org/meet-in-the-middle/)