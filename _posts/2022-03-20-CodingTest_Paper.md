---
title: "[Problems] 색종이"
excerpt: "paper paste by simple for loop"
date: 2022-03-20
layout: single
classes: wide
category:
    - problems
tag:
    - cpp
    - jungol 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-08-09/paper.jpg"
---

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

#define PAPER 1

int N;
int A[100 + 10];
int B[100 + 10];
int paper[102][102];
int ans;

void InputData() {
    cin >> N;
    for (int i = 0; i < N; i++) {
        cin >> A[i] >> B[i];
    }
}

void cover(int a, int b) {
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            paper[a + i][b + j] = PAPER;
        }
    }
}

const int dx[4] = {1, 0, -1, 0};
const int dy[4] = {0, -1, 0, 1};

void check(int y, int x) {
    int nx, ny;
    for (int i = 0; i < 4; i++) {
        ny = y + dy[i];
        nx = x + dx[i];

        // 가장자리 검사
        if (nx < 0) {
            ans++;
            continue;
        }
        if (ny < 0) {
            ans++;
            continue;
        }
        if (nx >= 100) {
            ans++;
            continue;
        }
        if (ny >= 100) {
            ans++;
            continue;
        }

        // 빈 곳 검사
        if (paper[ny][nx] == 0) {
            ans++;
        }
    }
}

int main() {
    ans = 0;

    InputData();// 입력받는 부분

    for (int i = 0; i < 102; i++)
        for (int j = 0; j < 102; j++)
            paper[i][j] = 0;

    // cover
    for (int i = 0; i < N; i++) {
        cover(A[i], B[i]);
    }

    // check round
    for (int i = 0; i < 100; i++) {
        for (int j = 0; j < 100; j++) {
            if(paper[i][j] == 1)
                check(i, j);
        }
    }

    cout << ans << endl;// 출력하는 부분
    return 0;
}
```