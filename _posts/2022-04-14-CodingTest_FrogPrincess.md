---
title: "[Coding Test] 개구리 공주"
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

# 개구리 공주

구현 문제의 극한. 감히 극한이라는 말을 썼다. 지금까지 풀어봤던 문제들 중에 제일 신박했던 유형. 중간에 함정도 두 개 있어 자칫 집중력이 흐트러지면 나락으로 치닫는 문제라 하겠다.

- 우상향 대각선은 합이 동일
- 우하향 대각선은 차가 동일
- 즉, (x, y) 좌표의 합 또는 차가 동일하면 그들은 같은 대각선 상에 위치함
- 각각을 합과 x 의 오름차순, 차와 x 의 내림차순으로 정렬하면 원하는 노드를 순서대로 접근 할 수 있다

```cpp
#include<iostream>
#include<algorithm>

#define MAXLEAF 100005
#define MAXJUMP	100005
using namespace std;

// 나뭇잎 구조체
struct LEAF {
	int x, y;
	int sum, dif;

	// A, B, C, D 방향으로 가장 가까운 LEAF pointer
	LEAF* link[4] = { NULL, };
};

enum _DIR {A, B, C, D};

// 나뭇잎
LEAF leafs[MAXLEAF];

// LEAF 기존 배열을 건드리지 않으면서, 임시 정렬을 위한 포인터 배열
LEAF* sortedLeaf[MAXLEAF];

// 식물 수, 점프 수 
int N, K;
char jumps[MAXJUMP];

// 정렬 기준
bool compareBC(LEAF* a, LEAF* b) {
	if (a->sum == b->sum) return a->x < b->x;
	return a->sum < b->sum;
}

bool compareAD(LEAF* a, LEAF* b) {
	if (a->dif == b->dif) return a->x < b->x;
	return a->dif < b->dif;
}

void Input() {
	// 입력
	for (int i = 0; i < N; i++) {
		cin >> leafs[i].x >> leafs[i].y;
		leafs[i].dif = (leafs[i].x - leafs[i].y);
		leafs[i].sum = (leafs[i].x + leafs[i].y);

		// 정렬에 사용할 배열
		sortedLeaf[i] = &leafs[i];
	}
}

void Build_Link() {
	// AD 순서 정렬
	sort(sortedLeaf, sortedLeaf + N, compareAD);
	for (int i = 0; i < N - 1; i++) {
		// 다음 leaf 와 합이 같으면 같은 대각선상에 위치
		if (sortedLeaf[i]->dif == sortedLeaf[i + 1]->dif) {
			// A : 0 
			// B : 1 
			// C : 2
			// D : 3
			sortedLeaf[i]->link[A] = sortedLeaf[i + 1];
			sortedLeaf[i + 1]->link[D] = sortedLeaf[i];
		}
	}

	// BC 순서 정렬
	sort(sortedLeaf, sortedLeaf + N, compareBC);
	for (int i = 0; i < N - 1; i++) {
		// 다음 leaf 와 차가 같으면 같은 대각선상에 위치
		if (sortedLeaf[i]->sum == sortedLeaf[i + 1]->sum) {
			// A : 0 
			// B : 1 
			// C : 2
			// D : 3
			sortedLeaf[i]->link[B] = sortedLeaf[i + 1];
			sortedLeaf[i + 1]->link[C] = sortedLeaf[i];
		}
	}
}

LEAF* Jump() {
	LEAF* cur = &leafs[0];
	LEAF* next;
	for (int i = 0; i < K; i++) {
		char dir = jumps[i];

		// 다음 위치로 점프
		if (cur->link[dir - 'A'] != NULL) {
			next = cur->link[dir - 'A'];

			// 현재 나뭇잎 없애기
			if(cur->link[A]) cur->link[A]->link[D] = cur->link[D];
			if(cur->link[D]) cur->link[D]->link[A] = cur->link[A];
			if(cur->link[B]) cur->link[B]->link[C] = cur->link[C];
			if(cur->link[C]) cur->link[C]->link[B] = cur->link[B];

			// 현재 위치 다음 위치로 이동
			cur = next;
		}
	}

	return cur;
}

int main(void) {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL); cout.tie(NULL);

	cin >> N >> K;
	cin >> jumps;
	LEAF* cur;
	Input();
	Build_Link();
	cur = Jump();

	cout << cur->x << ' ' << cur->y << '\n';

	return 0;
}
```