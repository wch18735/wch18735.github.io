---
title: "[Coding Test] 제곱수 만들기 (SWEA: 10965번)"
excerpt: "Implemenataion problem"
date: 2021-05-19
layout: single
classes: wide
category:
    - coding test
tag:
    - C++
    - Software Expert Academy
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 제곱수 만들기

이번 문제는 정말 어려웠습니다. 12번째 제출에 겨우 통과할 수 있었습니다. 핵심은 시간을 줄이는 데 있었습니다. 연산 시간을 줄일 수 있는 방법들을 아래에 나열해보면 다음과 같습니다.

- 모든 테스트케이스에 사용되는 에라토스테네스 체를 단 한 번만 계산합니다. **여러 번 사용되는 값은 한 번만 계산한다**는 원칙을 테스트 케이스 범위에도 적용할 수 있습니다.
- 에라토스테네스 체에서 소수들만을 추출해 하나의 리스트로 만듭니다. 그리하여 중간의 소수가 아닌 숫자를 탐색하는 시간을 줄입니다. **소수가 차지하는 비율**이 굉장히 적기 때문에 이들을 추출하는 것이 더욱 효율적입니다.
- 입력된 수가 소수인 경우 입력 값을 곧 답으로 출력합니다. **특정한 입력에 대해서 바로 답을 낼 수 있는 경우**에는 이를 적용합니다.
- 더욱 빠른 입출력을 활용합니다. 이 경우에 저는 `cin`, `cout` 대신 `scanf`, `printf`를 사용했습니다. 관련 내용은 [여기](https://www.geeksforgeeks.org/cincout-vs-scanfprintf/#:~:text=With%20synchronization%20turned%20off%2C%20the,does%20this%20at%20compile%20time.)에서 확인하실 수 있습니다.

주어진 $N$을 소인수분해 한 $k^p \cdot l^q \cdot m^r \ ...$ 에 대하여 $p, q, r, ...$ 값은 $2N + 1$ 또는 $2N$ 이 될 수 있습니다. 즉, 소인수가 짝수승인 경우엔 소인수의 제곱으로 계속 나누다보면 1이 됩니다. 반대로 소인수가 홀수승인 경우 자기 자신인 소인수 하나만을 남길 것입니다.

소인수의 제곱이 $N$을 넘어가는 경우부터는 $N$을 이루는 소인수더라도 하나씩만 존재하므로 반복을 종료합니다. 이 과정을 거쳐 마지막으로 남는 값들은 소인수들이 홀수인 경우입니다. 이 값들을 한 번씩 곱해주면 답이 될 수 있으므로, 연산을 거친 값을 답으로 출력합니다.

``` cpp
#include<iostream>
#include<algorithm>
#include<vector>
#include<math.h>
#include<cstdio>
 
using namespace std;
 
void solution(int test_case);
 
#define MAX_NUM 10000001
#define NOT_PRIME 1
#define PRIME 0
int era_che[MAX_NUM];
vector<int> prime_list;
 
int main(int argc, char* argv[])
{
    int T;
    scanf("%d", &T);
 
    // eratosthenes
    era_che[0] = 1;
    era_che[1] = 1;
    for (int i = 2; i <= (int)sqrt(MAX_NUM) + 1; i++)
    {
        if (era_che[i] == NOT_PRIME) continue;
        else
        {
            // insert I into prime_list
            prime_list.push_back(i);
 
            int idx = i + i;
            while (idx <= MAX_NUM - 1)
            {
                era_che[idx] = NOT_PRIME;
                idx += i;
            }
        }
    }
 
    // testcase solve with prime number cache
    for (int test_case = 1; test_case <= T; test_case++)
    {
        solution(test_case);
    }
 
    return 0;
}
 
void solution(int test_case)
{
    // input N
    int N;
    scanf("%d", &N);
 
    if (N == 1 || era_che[N] == PRIME)
    {
        cout << '#' << test_case << ' ' << N << '\n';
        return;
    }
 
    // factors
    vector<pair<int, int>> factors;
 
    int i = 0;
    int prime_square = prime_list[i] * prime_list[i];
 
    while (prime_square <= N)
    {
        if (N % prime_square == 0)
        {
            N /= prime_square;
        }
        else
        {
            i++;
            prime_square = prime_list[i] * prime_list[i];
        }
    }
 
    printf("#%d %d\n", test_case, N);
    //cout << '#' << test_case << ' ' << N << '\n';
}
```