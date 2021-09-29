---
title: "[Coding Test] 균형점 (SWEA: 1245)"
excerpt: "approximation by binary searching in margin of error"
date: 2021-09-29
layout: single
classes: wide
category:
    - coding test
tag:
    - c++
    - software expert academy
    - binary search
    - margin of error
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-09-23/coding_test.jpg"
---

# 균형점 (SWEA: 1245)

이번 문제는 이분탐색 개념을 이용해 값을 근사하는 과정을 다룹니다. [문제](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15MeBKAOgCFAYD)에서는 **N개**의 자성체들이 존재하는 공간에 존재하는 **N - 1개**의 평형지점을 구해야 합니다.

모든 자성체들은 일직선 상에 존재하는 것을 가정합니다. 따라서 어떠한 물체가 자성체들이 나열된 직선 상에 놓였을 때, 한 개의 자성체와 물체 사이에 작용하는 인력은 다음과 같이 계산됩니다.

$F = G \frac{m_1 * m_2}{d^2} $

이때, 좌표값의 오차가 $10^{-12}$보다 작아야 하는 조건이 존재합니다.

## 소스코드

```cpp
#include<iostream>
using namespace std;

struct Magnet {
	int position, weight;
};
int num;
Magnet magnetArr[10];

double bisection(int);

int main(int argc, char** argv) {
	
	// freopen("sample_input.txt", "r", stdin);
	
	int T, test_case;
	cin >> T;

	for (test_case = 1; test_case <= T; test_case++) {
		cin >> num;
		for (int i = 0; i < num; i++) cin >> magnetArr[i].position;
		for (int i = 0; i < num; i++) cin >> magnetArr[i].weight;

		cout.setf(ios::fixed, ios::floatfield);
		cout.precision(10);

		cout << '#' << test_case << ' ';
		for (int i = 0; i < num - 1; i++) {
			cout << bisection(i) << ' ';
		} cout << '\n';
	}

	return 0;
}

double bisection(int i) {
	double mid, low = magnetArr[i].position, high = magnetArr[i + 1].position;

	for (int turn = 0; turn < 100; turn++) {
		double leftforce = 0, rightforce = 0;
		mid = (low + high) / 2.0;

		double d;
		for (int left_idx = 0; left_idx <= i; left_idx++) {
			d = magnetArr[left_idx].position - mid;
			leftforce += (magnetArr[left_idx].weight * magnetArr[i].weight) / (d * d);
		}

		for (int right_idx = i + 1; right_idx <= num; right_idx++) {
			d = magnetArr[right_idx].position - mid;
			rightforce += (magnetArr[right_idx].weight * magnetArr[i].weight) / (d * d);
		}

		if (leftforce < rightforce) high = mid;
		else if (leftforce > rightforce) low = mid;
		else return mid;
	}

	return mid;
}
```

해당 코드가 허용 오차를 만족한다고 가정하는 이유는 `turn` 횟수입니다. 한 번의 Loop에 범위는 1/2 로 줄어듭니다. 따라서 어떠한 구간 $L$이 주어졌을 때, $2^{100}$의 정확도로 위치를 파악할 수 있습니다.

또한 `printf("%.10f)`와 같이 출력 형식자를 지정하는 방법 대신, 아래와 같은 코드가 사용되었습니다.

```cpp
cout.setf(ios::fixed, ios::floatfield);
cout.precision(10);
```