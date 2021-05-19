---
title: "[Coding Test] 군주제와 공화제 (SWEA: 10993번)"
excerpt: "Implemenataion problem"
date: 2021-05-19
layout: single
classes: wide
category:
    - coding test
tag:
    - C++
    - Brute Force
    - Software Expert Academy
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 군주제와 공화제

이 문제는 **Brute Force** + **구현** 문제라고 생각됩니다. 따라서 저는 `struct`를 사용해서 도시의 정보를 저장하는 구조체를 정의해 문제를 풀었습니다. 구현 문제의 핵심은 다음과 같습니다.

- 정확한 동작
- 효율적 연산과정

정확한 동작은 주어진 명세를 예외 상황을 생각하며 그대로 동작하도록 구현하는 것이 핵심입니다. 이 문제는 거리에 따라 도시가 위협되는지에 따른 경우를 분기하기가 까다롭다고 느꼈습니다. 실제로 여러 번의 **틀렸습니다**를 받았는데, 원인은 분기문이 모든 경우를 검사하지 못 한 것이었습니다. 

> **분기문이 모든 경우를 검사** 할 수 있도록 주석을 남기는 것이 하나의 포인트가 될 수 있을 것 같습니다.

모든 **city_info** 값들은 초기에 **K**이며 **follow = -1**로 초기화됩니다. 이후 자기 도시를 위협하는 적의 존재에 따라 위협당하는 정도인 **threaten**와 따르는 도시의 번호 **follow**, 현재 도시가 어떤 상태를 취하는 가를 나타내는 **state** 를 최신화합니다.

두 번째로 주의할 점은 효율적으로 연산하는 것입니다. 계속해서 사용되는 값인 **상대가 나를 위협하는 정도**와 **내가 상대를 위협하는 정도**는 한 번만 계산하도록 합니다. 분기문과 삽입 과정마다 나눗셈을 계산하는 것은 비효율적이기 때문입니다.

> **여러 번 사용되는 값**은 **한 번만 계산** 하도록 합니다.

끝으로 전역변수를 초기화하는 것이 중요합니다. 여러 테스트 케이스에 한 가지 전역변수 배열이 사용되므로 이전 테스트 케이스 값에 의해 틀린 답을 출력하는 경우가 발생할 수 있습니다. 

> 자신만의 방법으로 **전역변수 초기화**를 진행해주시면 조금 더 안정적인 동작을 보장할 수 있습니다.

```cpp
#include<iostream>
 
using namespace std;
 
void solution(int test_case);
int N;
 
struct city_info {
    int x;
    int y;
    double s;
    char state = 'K';
    int follow = -1;
    double threaten;
};
 
city_info cities[1001];
 
int main(int argc, char* argv[])
{
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
 
    int T;
    cin >> T;
 
    for (int test_case = 1; test_case <= T; test_case++)
    {
        solution(test_case);
    }
 
    return 0;
}
 
void input()
{
    cin >> N;
    int x, y, s;
 
    for (int i = 1; i <= N; i++)
    {
        cin >> x >> y >> s;
        cities[i].x = x;
        cities[i].y = y;
        cities[i].s = s;
    }
}
 
void solution(int test_case)
{
    // input
    input();
 
    // calculate relative with brute force
    int distance;
    for (int i = 1; i <= N; i++)
    {
        for (int j = i + 1; j <= N; j++)
        {
            if (cities[i].s == cities[j].s) continue;
 
            // distance between i and j
            int dist_x = cities[i].x - cities[j].x;
            int dist_y = cities[i].y - cities[j].y;
            distance = dist_x*dist_x + dist_y*dist_y;
             
            // threaten each other
            double i_to_j = cities[i].s / distance;
            double j_to_i = cities[j].s / distance;
 
            // check if j -> i threaten
            if (cities[i].s < j_to_i)
            {
                    // threaten as much power as cities[i].threaten
                    if (cities[i].threaten == j_to_i)
                    {
                        cities[i].state = 'D';
                        cities[i].follow = -1;
                    }
                    else if (cities[i].threaten < j_to_i)
                    {
                        // threaten newly
                        cities[i].state = 'T';
                        cities[i].follow = j;
                        cities[i].threaten = j_to_i;
                    }
            }
             
            if (cities[j].s < i_to_j)
            {
                if (cities[j].threaten == i_to_j)
                {
                    cities[j].state = 'D';
                    cities[j].follow = -1;
                }
                else if (cities[j].threaten < i_to_j)
                {
                    // threaten newly
                    cities[j].state = 'T';
                    cities[j].follow = i;
                    cities[j].threaten = i_to_j;
                }
            }
        }
    }
 
    int follow_node = 0;
    cout << '#' << test_case << ' ';
    for (int i = 1; i <= N; i++)
    {
        if (cities[i].state != 'T') cout << cities[i].state << ' ';
        else
        {
            follow_node = i;
            while (cities[follow_node].state == 'T') follow_node = cities[follow_node].follow;
            cout << follow_node << ' ';
        }
    }
    cout << '\n';
 
    // re-initialize
    for (int i = 1; i <= N; i++)
    {
        cities[i].follow = -1;
        cities[i].state = 'K';
        cities[i].threaten = 0.0;
    }
}
```