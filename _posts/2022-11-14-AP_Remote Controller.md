---
title: "[Algorithm Problem] 리모컨 (BAEKJOON: 1107)"
excerpt: "bfs"
date: 2022-11-13
layout: single
classes: wide
category:
    - algorithm problem
tag:
    - cpp
    - Beakjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 리모컨 (BAEKJOON: 1107)

리모컨 문제는 간단한 BFS에 고려해야 할 Edge case가 조금 섞인 유형으로 맞은 것 같은데 왜 틀렸는지 모르겠는 느낌을 받을 수 있다. 직접 반례를 찾아가는 연습하기에 좋다. 가령 0번 채널에 망가진 버튼이 하나도 없을 때, 정답은 1이지만 답을 0으로 출력하거나 100으로 출력하는 코드도 종종 보이곤 한다. 

## 풀이

```cpp
#include<iostream>
#include<queue>

using namespace std;

bool button[10];

int main(int argc, char* argv[]) {
	int CH, M;
	for (int i = 0; i < 10; i++) button[i] = true;

	cin >> CH;
	cin >> M;

	int t;
	for (int i = 0; i < M; i++) {
		cin >> t;
		button[t] = false;
	}

	int ans = CH <= 100 ? 100 - CH : CH - 100;
	int digits = CH != 0 ? 0 : 1;
	int ch = CH;
	while (ch != 0) {
		ch /= 10;
		digits += 1;
	}

	
	if (M == 0) {
		if (digits < ans)
			cout << digits;
		else 
			cout << ans;
		return 0;
	}

	queue<int> q;
	for (int b = 0; b < 10; b++) {
		if (button[b]) q.push(b);
	}

	while (!q.empty()) {
		int current_ch = q.front();
		q.pop();
		
		int pushed = current_ch != 0 ? 0 : 1;
		int t_current_ch = current_ch;
		while (t_current_ch != 0) {
			t_current_ch /= 10;
			pushed += 1;
		}

		if (pushed > digits + 1) continue; // 자리수가 2개 더 많은 경우
		pushed += current_ch <= CH ? CH - current_ch : current_ch - CH;

		if (pushed < ans) {
			ans = pushed;
		}

		if (current_ch == 0) continue;
		for (int i = 0; i < 10; i++) {
			if (button[i]) {
				q.push(current_ch * 10 + i);
			}
		}
	}

	cout << ans;

	return 0;
}
```
