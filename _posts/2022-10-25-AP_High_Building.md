---
title: "[Algorithm Problem] 고층 빌딩 (BAEKJOON: 1027)"
excerpt: "modeling"
date: 2022-10-25
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

# 고층 빌딩 (BAEKJOON: 1027)

이번 문제는 임의의 점 P(x, y) (단, 1 <= x <= 50,  1 <= y <= 10^9)가 평면 위에 최대 50개까지 주어질 때, 임의로 이은 두 점이 x축과 직교하는 선에 닿지 않는 개수를 모두 세고, 그 중 최대 개수를 구함으로써 풀 수 있다. 이는 기준 지점에서 가까운 방향부터 먼 방향으로 레이저 스캔을 진행할 때, 뒤에 가려진 부분은 제외하고 꼭지점 부분만 세는 방법을 물어보는 것과 같다. 

<img src="/_img/2022-10-25/laser scan.jpg">

여기서 핵심은 직교하는 선에 닿는가를 판단하는 방법이다. 이를 조금 더 단순화 해보자. 임의의 두 점 $P(x_1, y_1), Q(x_2, y_2)$가 있다고 가정하자. 이 때, $x_1 < x_2$ 이며 $y_1 < y_2$ 이다. 즉, $\overrightarrow {PQ}$ 기울기는 0보다 크다.

이 때, $x_1 < x_3 < x_2$인 위치에 점 $R(x_3, y_3)$이 주어졌다고 가정하자. 이 때, x축과 점 R을 이은 선분과 PQ가 만드는 직선이 만나지 않을 조건은 무엇일까? 간단하다. 점 R은 직선 PQ 아래 위치해야 한다. 즉, $\overrightarrow {PR}$이 이루는 기울기가 $\overrightarrow {PQ}$이 이루는 기울기보다 작으면 된다. 

이 굉장히 직관적이고 단순한 판단 방법을 구현하는 방법이 조금 까다로운 문제였다. 바깥쪽에서부터 검사하는 방법도 가능하고, 기준 점에서 멀어지는 방향으로 검사하는 방법도 가능하다. 여러 방법으로 풀 수 있으며, 50개가 최대이기 때문에 아마 대부분 제한 시간 안에 통과될 것이라 생각한다.

## 풀이1: 좌측, 우측을 나누어 판단

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

typedef long long int lli;
lli arr[60];
int N;

int watchable(lli _arr[], int curIdx) {
	int ans;
	long double criteriaGradient;
	long double gradient;

	ans = 0; criteriaGradient = 12345678910;

	// Left side
	for (int i = curIdx - 1; 0 < i; i--) {
		gradient = (double)(arr[curIdx] - arr[i]) / (curIdx - i);
		
		if (gradient < criteriaGradient) {
			ans += 1;
			criteriaGradient = gradient;
		}
	}

	criteriaGradient = -12345678910;

	// Right side
	for (int i = curIdx + 1; i <= N; ++i) {
		gradient = (double)(arr[curIdx] - arr[i]) / (curIdx - i);

		if (criteriaGradient < gradient) {
			ans += 1;
			criteriaGradient = gradient;
		}
	}

	return ans;
}

int main(int argc, char* argv[]) {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL); cout.tie(NULL);

	cin >> N;
	for (int i = 1; i <= N; i++) {
		cin >> arr[i];
	}

	int ans = 0;
	for (int i = 1; i <= N; i++) {
		ans = max(ans, watchable(arr, i));
	}

	cout << ans;

	return 0;
}
```

## 풀이2: 오른쪽으로 검사하며 점 개수 파악

```cpp
#include<iostream>

#define MIN_GRADIENT -9999999999

using namespace std;

typedef long long int lli;
lli arr[60];
int cnt[60];

int main(int argc, char* argv[]) {
	ios_base::sync_with_stdio(false);
	cin.tie(NULL); cout.tie(NULL);

	int N;
	cin >> N;
	for (int i = 0; i < N; i++) cin >> arr[i];

	double maxGradient, gradient;
	
	for (int i = 0; i < N - 1; i++) {
		maxGradient = MIN_GRADIENT;		// init
		for (int j = i + 1; j < N; j++) {
			gradient = (double)(arr[i] - arr[j]) / (i - j);
			if (maxGradient < gradient) {
				cnt[i] += 1;
				cnt[j] += 1;
				maxGradient = gradient;
			}
		}
	}

	int maxVal = 0;
	for (int i = 0; i < N; i++) maxVal = maxVal < cnt[i] ? cnt[i] : maxVal;

	cout << maxVal;

	return 0;
}
```