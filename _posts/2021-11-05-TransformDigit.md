---
title: "[Coding Test] 진법 변환 (후위 순회로 뒤부터 출력하기)"
excerpt: "backward printing by postorder"
date: 2021-09-23
layout: single
classes: wide
category:
    - coding test
tag:
    - cpp
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-09-23/coding_test.jpg"
---

# 문제

A진법 수 S를 입력 받아 B진법 수로 출력하는 프로그램을 작성하시오.

S에 사용되는 값은 0 ~ 9, A ~ Z이다.

각 조건은 다음과 같다.

- $2 \leq A, B \leq 36$
- $0 \leq S \leq 2^{63} - 1$

# 풀이

참고로 문제 강의는 54분으로 제일 길었는데, 처음으로 풀이 없이 푼 문제다.

그래서 아직 강의내용을 모름.

적용한 스킬은 아래와 같다.

- `typedef long long ll`
- 후위 순회 이용해 높은 자리수부터 출력하기
- char 형 데이터 자동 변환

# 소스코드

```cpp
#include<iostream>
#include<string>
 
using namespace std;
typedef long long ll;
ll A, B;
string S;
 
void print_answer(ll num) {
    if (num == 0) return;
    ll Mod = num % B;
 
    // 후위 순회
    print_answer(num / B);
 
    // 답출력
    if (Mod < 10) cout << Mod;
    else cout << (char)(Mod - 10 + 'A');
}
 
int main(int argc, char* argv[])
{
    // freopen("sample_input.txt", "r", stdin);
 
    while (true) {
        cin >> A;
        if (A == 0) break;
 
        cin >> S >> B;
        if (S.length() == 1 && S[0] == '0') {
            cout << 0 << '\n';
            continue;
        }
 
        int length = S.length();
        ll num = 0;
 
        for (int i = 0; i < length; i++) {
            if ('A' <= S[i])
                num = (num * A) + S[i] - 'A' + 10;
            else
                num = (num * A) + S[i] - '0';
        }
 
        // 10진수 변환 값 전달
        print_answer(num);
        cout << '\n';
    }
 
    return 0;
}
```