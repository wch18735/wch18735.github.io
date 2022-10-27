---
title: "[Algorithm Problem] 제곱수 찾기 (BAEKJOON: 1025)"
excerpt: "modeling"
date: 2022-10-27
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

# 제곱수 찾기 (BAEKJOON: 1025)

이 문제는 2차원 배열 형태로 나열된 숫자들 중 하나 이상을 선택해 만들 수 있는 수 중 가장 큰 제곱수를 구하는 문제다. 여기서 조건은 선택하는 행렬은 **등차수열**을 이루어야 한다는 것이다.

라이브러리 없이 제곱수를 구하는 방법으로 가장 먼저 떠오른 것은 에라토스테네스 체를 만들고, 소인수 분해를 진행한 다음 모든 수가 짝수인지 확인하는 것이었다. 다행히 정답이었지만, 나중에 제곱수인지 판단하는 기준에 라이브러리를 사용해보니 32ms에서 0ms로 바뀌긴 했다.

## 풀이1: 좌측, 우측을 나누어 판단

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

int arr[10][10];

typedef long long int lli;

// 에라토스테네스 체
bool era[100000];
void build() {
	fill_n(era, 100000, true);
	era[0] = era[1] = false;
	for (int i = 2; i < 100000; i++) {
		if (era[i] == false) continue;

		for (int m = 2; i * m < 100000; m++)
			era[i * m] = false;
	}
}

// check
bool check(lli num) {
	if (num < 2) return true;

	for (int div = 2; div < 100000; div++) {
		int cnt = 0;
		if (era[div] == false) continue;

		while (num % div == 0) {
			num /= div;
			cnt += 1;
		}

		if (cnt % 2 != 0) return false;
		if (num == 1) return true;
	}

	return false;
}

int main(int argc, char* argv[]) {
	ios_base::sync_with_stdio(false);
	cin.tie(nullptr); cout.tie(nullptr);

	int N, M;

	cin >> N >> M;

	// INPUT
	for (int i = 0; i < N; i++) {
		int T;
		cin >> T;

		for (int j = 0; j < M; j++) {
			arr[i][M - 1 - j] = T % 10;
			T /= 10;
		}
	}

	// 에라토스테네스's 체 만들기
	build();

	// CHECK
	lli ans = -1;
	for (int dr = -N; dr <= N; dr++) {
		for (int dc = -M; dc <= M; dc++) {
			if (dr == 0 && dc == 0) continue;

			for (int i = 0; i < N; i++) {
				for (int j = 0; j < M; j++) {
					lli num = 0;
					int ti = i;
					int tj = j;

					while (0 <= ti && ti < N && 0 <= tj && tj < M) {
						num *= 10;
						num += arr[ti][tj];
						ti += dr;
						tj += dc;

						if (check(num))
							ans = max(ans, num);
					}
				}
			}

		}
	}

	// PRINT
	cout << ans;

	return 0;
}
```
