---
title: "[Coding Test] 카드섹션"
excerpt: ""
date: 2022-04-14
layout: single
classes: wide
category:
    - coding test
tag:
    - cpp
    - baekjoon 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-08-09/paper.jpg"
---

# 카드섹션

`list` 자료구조를 적절히 이용하면 실행시간을 100배 줄일 수 있는 문제. 충분한 설계와 고민이 얼마나 중요한지 다시 한 번 깨닫게 된다. 문제가 주어질 때, 앞으로는 `가장 먼저 드는 생각은 오답` 이라는 마음으로 고민하고 또 고민하자.

- 정보 구성과 메모리 풀 / 관리 방법에 적용할 자료구조와 알고리즘 / 각 API 의 복잡도에 따른 피드백
- iterator 를 미리 저장했다가 바로 erase 해버리는 테크닉으로 O(1) 시간에 삭제를 수행할 수 있음
- 상대좌표를 절대좌표로 바꾸는 테크닉이 관건 (case 5 확인)
    - (tr, tc) 는 저장 타겟이 되는 좌표
    - 저장 타겟이란 주어지는 (r, c) 에서부터 4 * 4 만큼의 공간
    - 해당 타겟이 실제 섹션이 위치한 좌표와 겹치는 부분을 범위로 추가
    - 해당되는 좌표를 마지막으로 절대좌표로 변환해 저장
- 각 칸을 레이어링하는 개념은 3368ms 가 나오는 반면, 섹션들을 리스트로 잇는 개념을 적용하면 22ms 라는 결과를 얻는다
- 2차원 배열은 곧 1차원 배열에 (행 * 너비 + 열)로 나타낼 수 있음

```cpp
/**************************************************************
    Problem: 3148
    User: 1fes
    Language: C++
    Result: Success
    Time:22 ms
    Memory:4908 kb
****************************************************************/
 
 
#include<iostream>
#include<list>
using namespace std;
 
#define MAXSEC 10010
#define MAXLEN 1010
 
int N, M, T;
struct SECTION {
    int r, c, h, w;
    string color;
};
 
// 정보를 저장하는 곳은 따로 두기
SECTION section[MAXSEC];
 
// 추가 -> push_front
list<SECTION*> list_sec;
list<SECTION*>::iterator list_sec_iter[MAXSEC];
 
void init() {
    list_sec.clear();
}
 
int main(void) {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
 
    cin >> T;
    int cmd, id, r, c, w, h;
    for (int test_case = 0; test_case < T; test_case++) {
        cin >> N >> M;
        init();
        for (int m = 0; m < M; m++) {
            cin >> cmd;
            switch (cmd)
            {
            case 1: // 새로운 섹션 추가
                cin >> id;
                cin >> section[id].r >> section[id].c >> section[id].h >> section[id].w >> section[id].color;
                list_sec.push_front(&section[id]);
                list_sec_iter[id] = list_sec.begin();
                break;
 
            case 2: // 섹션 제거
                cin >> id;
                list_sec.erase(list_sec_iter[id]);
                break;
 
            case 3: // 마지막 순서로 섹션 조정
                cin >> id;
                //list_sec.splice(list_sec.begin(), list_sec, list_sec_iter[id]);
                list_sec.erase(list_sec_iter[id]);
                list_sec.push_front(&section[id]);
                list_sec_iter[id] = list_sec.begin();
                break;
 
            case 4: // 섹션 이동
                cin >> id >> r >> c;
                section[id].r = r;
                section[id].c = c;
                //list_sec.splice(list_sec.begin(), list_sec, list_sec_iter[id]);
                list_sec.erase(list_sec_iter[id]);
                list_sec.push_front(&section[id]);
                list_sec_iter[id] = list_sec.begin();
                break;
 
            case 5: // 4 * 4 크기 출력
                cin >> r >> c;
                auto iter = list_sec.begin();
                int cnt = 0; SECTION* s;
                int tr, tc; // target row, target column
                string pstr = "XXXXXXXXXXXXXXXX";
                while (iter != list_sec.end() && cnt < 16) {
                    s = *iter;
 
                    // 상대좌표 구하기
                    // ↑↓ : dy
                    // ←→ : dx
                    // a[r][c] --> a[y][x]
                    for (int dy = 0; dy < 4; dy++) {
                        tr = r + dy;
                        if (tr < s->r || tr >= s->r + s->h) continue;
                        for (int dx = 0; dx < 4; dx++) {
                            tc = c + dx;
                            if (tc < s->c || tc >= s->c + s->w) continue;
 
                            // 앞에서 채워졌는지 확인
                            if (pstr[dy * 4 + dx] != 'X') continue;
                            pstr[dy * 4 + dx] = s->color[(tr - s->r) * s->w + (tc - s->c)];
                            cnt++;
                        }
                    }
 
                    iter++;
                }
 
                for (char& ch : pstr) if (ch == 'X') ch = '0';
                cout << pstr << '\n';
                break;
            }
 
        }
    }
 
    return 0;
}
```