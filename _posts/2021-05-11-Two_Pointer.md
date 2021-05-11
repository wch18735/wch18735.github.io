---
title: "[Algorithms] Two Pointers Technique"
excerpt: "Technique that is typically used for searching pairs in a stored array"
date: 2021-05-11
layout: single
classes: wide
category:
    - algorithm
tag:
    - algorithms
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

## Two Pointer Algorithm (투 포인터 알고리즘)

정보를 담은 배열이 주어졌을 때, 조건을 만족하는 경우의 수를 찾기 위해서는 보통 **모든 경우를 탐색** 하는 방법을 가장 먼저 떠올립니다. 간단한 예제로 1~10 사이 임의의 정수 값으로 채워져있는 10칸의 배열이 주어졌다고 가정하겠습니다. 

<html>
    <head>
        <style>
            .number-container {
                display: flex;
                justify-content: center;
            }
            .number-item {
                margin: 0.5rem;
                padding: auto auto;
                background: #f3edd6;
                border: 3px solid #b59214;
                width: 40px; height: 40px;
                color: #b59214;
                font-size: 15px;
                text-align: center;
            }
        </style>
    </head>
    <body>
        <div class="number-container">
            <div class="number-item">10</div>
            <div class="number-item">2</div>
            <div class="number-item">4</div>
            <div class="number-item">7</div>
            <div class="number-item">1</div>
            <div class="number-item">6</div>
            <div class="number-item">9</div>
            <div class="number-item">8</div>
            <div class="number-item">3</div>
            <div class="number-item">4</div>
          </div>
    </body>
</html>
<br>

연속된 부분의 합이 7이 되는 경우의 수를 구할 때, 길이가 크지 않으므로 아래와 같이 코드를 작성할 수 있습니다.

``` python
if __name__=="__main__":
    answer = 0
    num_list = list(map(int, input().split(' ')))

    for i in range(len(num_list) - 1):
        for j in range(len(num_list) - 1):
            sum = 0

            for idx in range(i, j + 1):
                sum += num_list[idx]

            if sum == 7:
                answer += 1

    print(answer)
```

위의 코드는 3중 for문을 돌게 되는데 10개의 경우 그 연산 수가 **1,000번** 밖에 되지 않아 오래 걸리지 않습니다. 그런데 만약 배열의 수가 10,000개로 늘어난다면 어떻게 될까요? 연산 수는 $10^{12}$가 되어 결과를 확인하는 데 많은 시간이 걸릴 것입니다.  
  
이런 경우 활용하는 것이 **Two Pointer Technique** 입니다. 위의 3중 for문도 크게 보면 두 개의 포인터가 순차적으로 움직인다고 생각하시면 이해가 조금 더 빠를 것 같습니다. 배열의 두 지점을 나타내는 **pointer**를 두고 조건에 따라 포인터를 이동시키며 값을 구합니다. 이때, 아이디어는 아래와 같습니다.

1. 부분합 배열의 **시작(start_pointer)** 과 **끝(end_pointer)** 을 모두 0번 index를 가리키게 합니다.
2. 초기 값을 부분합에 넣습니다.
3. 현재 부분합이 **Target Number** 와 같다면
    a. 정답 경우의 수를 하나 더합니다.
    b. **ARRAY[start_pointer]** 값을 부분합에서 빼줍니다.
    c. **start_pointer** 값을 하나 더해 오른쪽으로 이동시킵니다.
4. 현재 부분합이 **Target Number**보다 작다면
    a. **end_pointer** 값을 하나 더해 오른쪽으로 이동시킵니다.
    b. **ARRAY[end_pointer]** 값을 부분합에 더해줍니다.
5. 현재 부분합이 **Target Number**보다 크다면
    b. **ARRAY[start_pointer]** 값을 부분합에서 빼줍니다.
    c. **start_pointer** 값을 하나 더해 오른쪽으로 이동시킵니다.
6. **3**~**5** 과정을 **end_pointer**가 마지막 index를 넘어갈 때까지 반복합니다.

모든 수가 양수이기 때문에 **start_pointer**를 이동시키면 부분합이 줄어들고, **end_pointer**를 이동시키면 부분합이 커집니다. 

또한 이 과정에서 **start_pointer** 에서 **end_pointer**까지 for문을 통해 부분합을 구하는 방법도 가능하나 기존에 구해놓은 부분합과 공통된 부분은 유지하며 포함되지 않는 부분은 지우고 포함되는 부분은 새로 추가하는 방식을 통해 연산량을 줄일 수 있습니다.

이를 활용한 문제들을 아래에서 볼 수 있습니다. 문제를 풀어보며 다양한 상황에서 활용할 수 있는 **Two Pointer** 기법을 익히면 좋을 것 같습니다.

### Two Pointer 문제
- [3273:두 수의 합](https://www.acmicpc.net/problem/3273)
- [2470:두 용액](https://www.acmicpc.net/problem/2470)
- [1806:부분합](https://www.acmicpc.net/problem/1806)
- [1644:소수의 연속합](https://www.acmicpc.net/problem/1644)