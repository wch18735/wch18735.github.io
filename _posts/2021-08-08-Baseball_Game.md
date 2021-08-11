---
title: "[Coding Test] 야구 (BAEKJOON: 17281)"
excerpt: "baseball game implementation: brute force"
date: 2021-08-08
layout: single
classes: wide
category:
    - coding test
tag:
    - python
    - baekjoon
    - dynamic programming
    - brute force
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-08-09/baseball.jpg"
---

# 야구 (BAEKJOON: 17281)

[이번 문제](https://www.acmicpc.net/problem/17281)는 주어진 게임을 규칙에 따라 구현하고, 결과값을 탐색하면 풀 수 있습니다. 먼저 기본적으로 야구를 설명해줍니다. 아래는 그 내용입니다.

- 야구는 9명으로 이루어진 두 팀이 공격과 수비를 번갈아 하는 게임
- 하나의 이닝은 **공격**과 **수비**로 이루어짐
- **N이닝** 동안 게임을 진행함
- 한 이닝에 **3 OUT**이 발생하면 이닝이 종료되며 공수를 교대함

아래는 세부적인 경기 규칙입니다.

- 두 팀은 경기가 시작하기 전까지 타순을 정함
- 경기 중에는 타순을 **변경할 수 없음**
- 9번 타자까지 공을 쳤는데 **3 OUT**이 발생하지 않으면 이닝은 끝나지 않음
- **3 OUT**이 발생할 때까지 타순은 계속해서 순환함
- 타순은 이닝이 변경되어도 계속해서 유지됨 
- 첫 이닝 이후 다음 이닝이 시작될 때, 공수 전환 직전 타순을 계속해서 이어감

문제에 등장하는 감독은 각 선수가 각 이닝에서 어떤 결과를 얻는지 미리 알고 있습니다. 그래서 1번부터 9번까지 등번호를 가진 선수를 적절하게 섞어 최대 점수를 얻을 타순을 만드려 합니다.

구해야하는 것은 **선수를 적절하게 섞어 얻은 타순을 통해 얻을 수 있는 최대 점수**입니다.

## 입력

- 첫째 줄에 이닝 수 $N (2 \leq N \leq 50)$ 이 주어집니다. 
- 둘째 줄부터 $N$개의 줄에는 각 선수가 각 이닝에서 얻는 결과가 1번 이닝부터 N번 이닝까지 순서대로 주어집니다. 
- 이닝에서 얻는 결과는 9개의 정수가 공백으로 구분됩니다.

각 결과가 의미하는 정수는 다음과 같습니다.

- 안타: 1
- 2루타: 2
- 3루타: 3
- 홈련: 4
- 아웃: 0

각 이닝에는 아웃을 기록하는 타자가 적어도 한 명 존재합니다.

## 예제 입력

```
2
4 0 0 0 0 0 0 0 0
4 0 0 0 0 0 0 0 0
```

## 예제 출력

```
1
```

## 풀이

풀이에 필요한 Python 팁과 해결과정을 설명해보겠습니다.

### 풀이 TIP

먼저 한 가지 팁은 **파이썬의 변수 할당과 Swap 구현**입니다. 파이썬은 **C++** 과는 다르게 스왑 함수를 작성할 필요가 없습니다. 다음과 같이 작성하면 됩니다.

```python
# 콤마(,)를 이용한 변수 할당
a, b = 0, 2

# 콤마(,)를 이용한 스왑(Swap)
a, b = b, a
```

두 번째 팁은 `from itertools import permutations` 로 사용할 수 있는 `permutations` 입니다. 이 모듈을 이용해 순열을 만들어 낼 수 있습니다. 사용법은 다음과 같습니다.

```python
from itertools import permutations

player_idx_list = [i for i in range(1, 9)]

for player_order in permutations(player_idx_list, 8):
    player_order = list(player_order)

    # .... #
    # 생략 #
    # .... # 
```

세 번째 팁은 **리스트 슬라이싱(List Slicing)** 입니다. 리스트는 아래와 같이 원소에 접근할 수 있습니다.

```python
num_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# LIST[start: end: step] => [start, end)

print(num_list[0:5])    # (1, 2, 3, 4, 5)
print(num_list[5:10])   # (6, 7, 8, 9, 10)
print(num_list[0:10:2]) # (1, 3, 5, 7, 9)
```

마지막으로 리스트는 + 연산자에 리스트 요소들을 저장할 수 있게 오버라이딩 된 것처럼 동작한 것입니다. `[1, 2, 3]` 과 `[4, 5, 6]` 을 원소들만 합치고 싶다면 `+` 연산자를 이용하면 간단하게 해결할 수 있습니다.

### 해결과정

저는 다음과 같이 게임을 구현했습니다.

- **GAME** 에 각 이닝에 등번호에 따른 기록 저장
- `permutations(player_idx_list, number)` 를 이용해 모든 타순을 생성
- `player_order[:3] + [0] + player_order[3:]` 을 통해 4번 타순에 **등번호 1번 고정**
- 고정된 현재 타석 번호 `current_idx` 선언하고 초기값 **0** 대입

이렇게 기본적인 세팅을 끝낸 뒤, 저는 INNING 만큼 for 문을 돌았습니다. 그리고 각 이닝별로 **Out count** 가 3이 될 때까지 **while** 내에서 게임을 진행시켰습니다.

각 게임마다 진루가 어떻게 이뤄지는가를 표현해주면 됩니다. **base1, base2, base3**은 각 루를 뜻하고 그 위에 선수가 있다면 1을 선언하고, 없다면 0을 대입합니다. 당연히 처음 이닝이 시작될 때에는 모든 값이 0으로 초기화됩니다.

```python
base1, base2, base3 = 0, 0, 0
```

그리고 주어진 기록마다 값을 **score 에 덧셈** 해주면 됩니다. 이를 코드로 표현하면 아래와 같습니다.

```python
    # 아웃일 때
    if GAME[inning][player_idx] == 0:
        out_count += 1

    elif GAME[inning][player_idx] == 1:
        # 3루 값을 더해줌
        score += base3

        # 한 칸 씩 진루
        base1, base2, base3 = 1, base1, base2

    elif GAME[inning][player_idx] == 2:
        # 2,3루 값을 더해줌
        score += (base3 + base2)

        # 2칸 씩 진루
        base1, base2, base3 = 0, 1, base1

    elif GAME[inning][player_idx] == 3:
        # 1,2,3루 값을 더해줌
        score += (base3 + base2 + base1)

        # 3칸 씩 진루
        base1, base2, base3 = 0, 0, 1

    elif GAME[inning][player_idx] == 4:
        # 필드 위 모든 값 더함
        score += (base3 + base2 + base1 + 1)

        # 필드 위 아무도 없음
        base1, base2, base3 = 0, 0, 0
```

그리고 현재 타순의 위치를 모듈로 연산으로 순환시키면 답을 구할 수 있습니다. 

```python
current_idx = (current_idx + 1) % 9
```

이 문제는 모든 경우를 탐색하는 것으로 **Brute force** 알고리즘으로 분류되어 있습니다. 아래는 전체적인 코드입니다.

```python
from itertools import permutations

INNING = int(input())
GAME = [list(map(int, input().split(' '))) for i in range(INNING)]

def main():
    player_idx_list = [i for i in range(1, 9)]

    answer = 0
    for player_order in permutations(player_idx_list, 8):
        # player 순서 뽑음
        player_order = list(player_order)
        player_order = player_order[:3] + [0] + player_order[3:]

        # 최종 점수
        score = 0

        # 현재 타석 번호
        current_idx = 0

        # 이닝 별 경기
        for inning in range(INNING):
            # 1번, 2번, 2번 베이스 값
            base1, base2, base3 = 0, 0, 0

            # out count 초기화
            out_count = 0

            while out_count < 3:
                player_idx = player_order[current_idx]

                # 아웃일 때
                if GAME[inning][player_idx] == 0:
                    out_count += 1

                elif GAME[inning][player_idx] == 1:
                    # 3루 값을 더해줌
                    score += base3

                    # 한 칸 씩 진루
                    base1, base2, base3 = 1, base1, base2

                elif GAME[inning][player_idx] == 2:
                    # 2,3루 값을 더해줌
                    score += (base3 + base2)

                    # 2칸 씩 진루
                    base1, base2, base3 = 0, 1, base1

                elif GAME[inning][player_idx] == 3:
                    # 1,2,3루 값을 더해줌
                    score += (base3 + base2 + base1)

                    # 3칸 씩 진루
                    base1, base2, base3 = 0, 0, 1

                elif GAME[inning][player_idx] == 4:
                    # 필드 위 모든 값 더함
                    score += (base3 + base2 + base1 + 1)

                    # 필드 위 아무도 없음
                    base1, base2, base3 = 0, 0, 0

                # 다음 player index
                current_idx = (current_idx + 1) % 9

        # 모든 경기 종료 후
        answer = max(answer, score)

    print(answer)


if __name__=="__main__":
    main()

# while Ture: -> while out_count < 3: 시간초과 통과
# 시간이 촉박한 경우 여러 if 분기문 역시 시간초과의 한 이유가 될 수 있음
# brute force 의 경우 for 문을 통한 전체 검색으로 풀이 가능!!
```

## 오답 풀이

아래는 오답 풀이입니다. 처음 문제를 풀면서 DFS를 통해 모든 경우를 탐색하려 했습니다. 정확도는 만족했으나 시간초과를 해결하지 못 했습니다.

```python
# 이닝 수
N = int(input())

# 경기 기록
record = [list(map(int, input().split(' '))) for _ in range(N)]

# 최종 수열 (1번은 항상 4번 타자)
order_array = [0 for i in range(9)]
order_array[0] = 4

# answer
answer = 0
result = 0

# move base
PLAYER = True
EMPTY = False
base_state = [PLAYER, EMPTY, EMPTY, EMPTY]

# score
score = [0 for i in range(50)]

def move_base(hit_number):
    global result

    for base_number in range(3, -1, -1):
        if base_state[base_number] == PLAYER:
            # 홈으로 돌아온다면
            if base_number + hit_number >= 4:
                result += 1
                base_state[base_number] = EMPTY
            # 홈으로 돌아오지 못 한다면
            else:
                # 안타만큼 진루하고, 해당 칸은 비워둠
                base_state[base_number + hit_number] = PLAYER
                base_state[base_number] = EMPTY

# play game
def play_game():
    global base_state, answer, result
    player_order_list = list()

    # (선수 번호, 타순)
    for player_idx, player_order in enumerate(order_array):
        player_order_list.append((player_idx, player_order))

    # 타순으로 정렬
    player_order_list.sort(key=lambda x: x[1])

    # 타순에 따라 경기 진행
    current_player_idx = 0
    inning = 0
    out_count = 0

    while True:
        # 모든 이닝 종료
        if inning == N:
            break

        # 플레이어 뽑기
        base_state[0] = PLAYER
        player_idx = player_order_list[current_player_idx][0]   # player idx
        hit_number = record[inning][player_idx]
        if hit_number == 0:
            out_count += 1

            # out count 가 3개이면 이닝 종료 (아웃 초기화, 다음 이닝, 다음 선수부터 진행, 베이스 초기화)
            if out_count == 3:
                out_count = 0
                inning += 1
                base_state = [PLAYER, EMPTY, EMPTY, EMPTY]
        else:
            # move base
            move_base(hit_number)

        # 다음 선수 뽑음
        current_player_idx += 1
        current_player_idx %= 9

    # 모든 이닝 종료 후
    if result > answer:
        answer = result
        result = 0
    else:
        result = 0

def dfs(cnt):
    global answer

    # cnt == 10 일 때, 1 ~ 9로 채워진 수열 완성
    if cnt == 10:
        play_game()
        return

    # cnt == 4번은 이미 존재
    if cnt == 4:
        dfs(cnt + 1)
        return

    # 현재 순서가 채워지지 않은 칸 탐색
    for i in range(1, 9):
        if order_array[i] != 0:
            continue

        order_array[i] = cnt
        dfs(cnt + 1)
        order_array[i] = 0

def main():
    dfs(1)
    print(answer)

if __name__=="__main__":
    main()
```