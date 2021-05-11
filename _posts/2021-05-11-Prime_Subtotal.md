---
title: "[Algorithm Problem] 소수의 연속합 (BAEKJOON: 1644번)"
excerpt: "Two Pointer Technique"
date: 2021-05-11
layout: single
classes: wide
category:
    - algorithm problem
tag:
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

# Prime Number Subtotal (부분합)

이 문제는 어떠한 자연수 N$(1 \le N \le 4,000,000)$이 주어졌을 때, 연속된 소수의 합으로 표현할 수 있는 경우의 수를 구하는 문제입니다. 언뜻 보면 투 포인터를 이용한 가장 기본적인 형태로 생각할 수 있어 쉬워보이나 **연속된 소수 배열**을 구해내기가 까다로울 수 있습니다. 그치만 문제를 많이 풀어보신 분들은 곧바로 **에라토스테네스 체**를 떠올리실 수 있을거라 생각합니다.  
  
에라토스테네스 체를 구한 뒤, 연속된 소수 배열만을 추출해 이전의 투 포인터 알고리즘과 결합하면 문제를 쉽게 풀어낼 수 있습니다.  
  
### 소스코드

``` python
if __name__=="__main__":
    N = int(input())

    # 에라토스테네스 체
    era_che = [True for _ in range(N + 1)]
    era_che[0], era_che[1] = False, False

    # 소수 시작
    num = 2

    # 에라토스테네스 체
    while num * num <= N:
        idx = 2
        while num * idx <= N:
            era_che[num * idx] = False
            idx += 1
        num += 1

    # 소수만 모으기 (index error 방지용 float('inf'))
    era_che = [idx for idx, chk in enumerate(era_che) if chk is True] + [float('inf')]

    # 부분합 경우의 수 계산
    start_pointer = 0
    end_pointer = 0
    partial_sum = era_che[end_pointer]

    # 답
    answer = 0

    while end_pointer < len(era_che) - 1:
        if partial_sum < N:
            end_pointer += 1
            partial_sum += era_che[end_pointer]
        elif partial_sum == N:
            answer += 1
            partial_sum -= era_che[start_pointer]
            start_pointer += 1
        else:
            partial_sum -= era_che[start_pointer]
            start_pointer += 1

    print(answer)
```
