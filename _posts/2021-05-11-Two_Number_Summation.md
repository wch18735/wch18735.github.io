---
title: "[Algorithm Problem] 두 수의 합 (BAEKJOON: 3273번)"
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

# Two Nnumber Summation

이 문제는 **서로 다른** 양의 정수 $a_1, a_2, ... , a_n$ 으로 이루어진 수열에 대해 **$a_i + a_j = x (1 \le i < j \le n)$** 을 만족하는 $(a_i, a_j)$을 구하는 문제입니다. 수열의 크기 $n$과 구해야하는 값 $x$의 범위는 각각 $(1 \le n \le 100000, 1 \le x \le 2000000)$입니다.  
  
이 문제는 투 포인터 기법을 이용해 풀 수 있습니다. 과정은 아래와 같습니다.
- 주어진 숫자 배열을 오름차순으로 정렬합니다.
- **start_pointer = 0** 과 **end_pointer = 배열길이 - 1** 을 초기 인덱스로 설정합니다.
- 각 포인터가 가리키는 값의 합이 타깃보다 크다면 **end_pointer**를 한 칸 왼쪽으로 옮깁니다.
- 각 포인터가 가리키는 값의 합이 타깃보다 작다면 **start_pointer**를 한 칸 오른쪽으로 옮깁니다.
- 각 포인터가 가리키는 값의 합이 타깃과 같다면 경우를 1 더해주고 **start_pointer**와 **end_pointer**를 각각 만나는 방향으로 옮깁니다.
- 이 과정을 두 포인터가 만날 때까지 진행합니다.

정답일 때 포인터를 두 개 모두 움직이는 이유는 문제의 조건이 **서로 다른** 양의 정수이기 때문에 한 쪽을 움직이더라도 다음 한 쪽이 필연적으로 움직이기 때문입니다. 이 과정을 생략하고 코드 길이를 줄일 수 있으나 저는 연산 과정을 하나 줄였습니다.  
  
## 소스코드

```python
if __name__=="__main__":
    N = int(input())

    num_list = list(map(int, input().split(' ')))
    num_list.sort(key=lambda x: x)

    start_pointer = 0
    end_pointer = len(num_list) - 1

    target_num = int(input())

    answer = 0

    while start_pointer < end_pointer:
        # 두 수 쌍 합 구하기
        partial_sum = num_list[start_pointer] + num_list[end_pointer]

        # 판단
        # 서로 다르기 때문에 다음 수가 같은 것인지 판단할 필요 없음
        if partial_sum < target_num:
            start_pointer += 1
        elif partial_sum == target_num:
            answer += 1
            start_pointer += 1
            end_pointer -= 1
        elif partial_sum > target_num:
            end_pointer -= 1

    print(answer)
```