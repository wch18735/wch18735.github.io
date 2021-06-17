---
title: "[Algorithm Problem] 전깃줄 (SWEA: 10585번)"
excerpt: "For loop and Longest Increasing Subsequence"
date: 2021-06-19
layout: single
classes: wide
category:
    - algorithm problem
tag:
    - python
    - SWEA
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 전깃줄

이 문제는 두 전봇대에 연결된 여러 개의 전선들이 몇 개의 교점을 이루는지를 확인하는 문제입니다. 각 전봇대에 연결된 높이쌍은 최대 1,000개가 주어집니다. 그렇기 때문에 **이중 for 문**을 이용해서도 쉽게 풀어낼 수 있습니다.

```python
if __name__=="__main__":
    T = int(input())

    for test_case in range(1, T + 1):
        N = int(input())

        lines = []
        for _ in range(N):
            a, b = map(int, input().split(' '))
            lines.append((a, b))

        answer = 0
        lines_length = len(lines)
        for i in range(lines_length):
            for j in range(lines_length):
                if i == j:
                    continue

                if (lines[i][0] - lines[j][0]) * (lines[i][1] - lines[j][1]) < 0:
                    answer += 1

        answer = answer // 2

        print("#{} {}".format(test_case, answer))
```

`꼬여있는 위치`를 표현하기 위해 저는 아래와 같은 식을 이용했습니다.

```python 
if (lines[i][0] - lines[j][0]) * (lines[i][1] - lines[j][1]) < 0:
```

줄 A와 줄 B가 꼬여있는 경우 어느 쪽이든 하나는 높고, 하나는 낮기 때문에 같은 순서로 뺄셈을 해주면 그 두 결과의 부호는 서로 다릅니다. 같은 높이는 문제에서 주이지지 않으므로 0보다 작은 값을 확인하여 `꼬여있음`을 판단할 수 있습니다.

저는 여기서 **LIS (가장 긴 증가하는 수열)** 개념을 사용해보고 싶었습니다. N의 개수가 늘어나게 되면 1초의 제한시간을 만족하지 못 할 수도 있습니다. **LIS**를 구한 뒤 해당 수열에 포함되지 않는 줄의 위치를 이용해 판단할 수 있습니다.

아래는 해당 알고리즘으로 풀어낸 방법입니다. 아쉽게도 Testcase는 통과하나 제출 시 Runtime Error가 발생합니다. 어떤 문제가 있는지 아시는 분은 댓글에 남겨주시면 정말 감사하겠습니다.

```python
from bisect import bisect_left

def main():
    T = int(input().strip())

    for test_case in range(1, T + 1):
        N = int(input().strip())

        L = []

        for _ in range(N):
            a, b = map(int, input().strip().split(' '))
            L.append((a, b))

        # 오름차순으로 정리
        L.sort(key=lambda x: x[0])
        Line = list(zip(*L))[1]

        # LIS 찾기
        LIS = []
        DP = [-1 for i in range(len(Line))]

        for idx, element in enumerate(Line):
            if len(LIS) == 0:
                # 비어있는 경우
                LIS.append(element)
                DP[idx] = 0
            else:
                if LIS[-1] < element:
                    # LIS 마지막에 추가
                    DP[idx] = len(LIS)
                    LIS.append(element)
                else:
                    # 삽입될 위치를 찾음
                    pos = bisect_left(Line, element)
                    DP[idx] = pos
                    LIS[pos] = element

        num = len(LIS) - 1

        DP = DP[::-1][:]
        for idx, elem in enumerate(DP):
            if elem == num:
                DP[idx] = -1
                num -= 1
        DP = DP[::-1][:]

        answer = 0
        dp_length = len(DP)
        for idx, elem in enumerate(DP):
            if elem != -1:
                answer += dp_length - (idx + 1)

        print("#{} {}".format(test_case, answer))
                    
if __name__=="__main__":
    main()
```