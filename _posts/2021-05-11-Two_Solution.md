---
title: "[Algorithm Problem] 두 용액 (BAEKJOON: 2470번)"
excerpt: "Two Pointer Technique"
date: 2021-05-11
layout: single
classes: wide
category:
    - algorithm problem
tag:
    - C++
    - Python
    - Two Pointer
    - Dynamic Programming
    - Baekjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Two Solutions (두 용액)

이 문제는 이전에 풀었던 [두 수의 합](https://wch18735.github.io/algorithm%20problem/Two_Number_Summation/) 문제와 비슷한 풀이 과정을 거칩니다. 수를 정렬하고, 포인터를 이용해 배열의 각 원소를 **순차적으로** 접근하여 문제를 해결합니다.  
  
문제의 입력은 다음과 같습니다.
- 전체 용액 수 $2 \le N < 100000$
- 용액 값의 범위 $-1,000,000,000 \le x \le 1,000,000,000$
- 주어지는 용액은 산성용액 또는 알칼리로만 주어질 수도 있습니다.
  
이렇게 주어진 정수 배열에서 고른 두 수의 합의 절대값이 0에 가장 가까운 한 쌍을 찾는 것이 요구 조건입니다. 이중 for문을 이용해 모든 조건을 확인하는 방법도 가능하지만 $10^{12}$에 해당하는 연산을 거쳐야 하기 때문에 조금 더 효율적인 방법이 필요합니다. 물론 프로그램의 제한시간이 충분하다면 간단하고 직관적인 코드를 사용할 것입니다.  
  
저는 이 문제를 다음과 같이 풀었습니다.
- 주어진 용액을 오름차순으로 정렬합니다.
- 시작점과 끝점의 포인터를 지정합니다.
- **초기 혼합용액 최소값**을 $2,000,000,000$으로 지정합니다.
- **초기 혼합용액 쌍**에 **좌측점**과 **우측점**의 포인터를 대입합니다.
- 포인터가 가리키는 값들의 합이 음수일 때
    - 절대값이 혼합용액 최소값보다 작다면
        - 혼합용액 최소값을 수정합니다.
        - 혼합용액 쌍에 현재 포인터를 대입합니다.
        - 두 포인터 값을 가까워지도록 한 칸씩 이동합니다.
    - 절대값이 혼합용액 최소값보다 크거나 같다면
        - 좌측점을 한 칸 오른쪽으로 이동합니다.
- 포인터가 가리키는 값들의 합이 양수일 때
    - 절대값이 혼합용액 최소값보다 작다면
        - 혼합용액 최소값을 수정합니다.
        - 혼합용액 쌍에 현재 포인터를 대입합니다.
        - 두 포인터 값을 가까워지도록 한 칸씩 이동합니다.
    - 절대값이 혼합용액 최소값보다 크거나 같다면
        - 우측점을 한 칸 왼쪽으로 이동합니다.
- 포인터가 가리키는 값들의 합이 0일 때
    - 좌측값과 우측값을 순서대로 저장하고 탐색을 끝냅니다.
- 이 과정을 좌측점과 우측점이 만날 때까지 진행합니다.

이 문제는 두 합이 음수냐, 양수냐에 따라 경우를 나눠줘야 했기에 조금 까다로웠습니다. 그치만 **정렬**과 함께 **두 개의 값**에 **순차적 접근**하는 것을 키워드로 문제를 바라보면 투 포인터를 떠올릴 수 있고 비교적 쉽게 풀이 방법을 떠올릴 수 있었습니다.

### 소스코드

``` python
if __name__=="__main__":
    N = int(input())

    # 용액
    liquid_list = list(map(int, input().split(' ')))

    # 정렬
    liquid_list.sort(key=lambda x: x)

    left_pointer = 0
    right_pointer = len(liquid_list) - 1

    # 최대 값
    INF = 2 << 31
    min_rate = INF

    # 최소 쌍
    minimum_pair = [left_pointer, right_pointer]

    while left_pointer < right_pointer:
        # 용액 합성
        acid_rate = liquid_list[left_pointer] + liquid_list[right_pointer]

        if acid_rate == 0:
            minimum_pair[0] = liquid_list[left_pointer]
            minimum_pair[1] = liquid_list[right_pointer]
            break
        elif acid_rate < 0:
            if abs(acid_rate) < min_rate:
                min_rate = abs(acid_rate)
                minimum_pair[0] = liquid_list[left_pointer]
                minimum_pair[1] = liquid_list[right_pointer]
            left_pointer += 1
        elif acid_rate > 0:
            if abs(acid_rate) < min_rate:
                min_rate = abs(acid_rate)
                minimum_pair[0] = liquid_list[left_pointer]
                minimum_pair[1] = liquid_list[right_pointer]
            right_pointer -= 1

    print(*minimum_pair)
```