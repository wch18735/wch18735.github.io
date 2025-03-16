---
title: "[Problems] 관리 난항"
excerpt: ""
date: 2022-04-14
layout: single
classes: wide
category:
    - problems
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

# 관리 난항

이 문제는 [백준:9367](https://www.acmicpc.net/problem/9367) 에서 풀 수 있다.

- `if ~ else` 보다 `switch ~ case` 가 기계어 수준에서 더 빠르다
- `enum` 키워드를 통해 단어에 열거형 의미를 부여할 수 있다
- 복잡한 문제는 아래 세 가지를 무조건 고려해 설계한다
    - 정보를 저장할 메모리 풀
    - 접근 정보를 관리할 자료 구조
    - 연산에 적합한 알고리즘
- 명령어에 따른 복잡도는 항상 최악을 고려한다

```cpp
#include<iostream>
#include<string>
#include<unordered_map>
#include<algorithm>
#include<cmath>
using namespace std;

#define MAxCAR 510
#define MAXAGENT 510

// empty, pick-up, return, accident
enum STATE { p, r, a };
typedef long long int lli;

// 같은 차 종에 대해 여러 대 대여 가능
struct CAR {
	string name;
	int price;
	int rent;
	int killo;
};

struct AGENT {
	string name;
	lli total = 0;
	lli repair = 0;
	bool inconsistent = false; // 여러 번 대여, 미반납
	CAR* car = NULL;

	bool operator<(AGENT a) const {
		return this->name < a.name;
	}
};

CAR car[MAxCAR];
AGENT agent[MAXAGENT];
unordered_map<string, AGENT*> agtTable;
unordered_map<string, CAR*> carTable;
int N, M, agtNum;
void Input() {
	cin >> N >> M;
	agtNum = 0;
	agtTable.clear();
	carTable.clear();

	// 차 입력
	for (int i = 0; i < N; i++) {
		cin >> car[i].name >> car[i].price >> car[i].rent >> car[i].killo;
		carTable.insert({ car[i].name, &car[i] });
	}
}

void Process() {
	int time, distance, broken;
	string agt_name;
	string car_name;
	char cmd;
	AGENT* _agt; CAR* _car;

	for (int i = 0; i < M; i++) {
		cin >> time >> agt_name >> cmd;
		// agent 등록
		if (agtTable.count(agt_name) == 0) {
			agent[agtNum].total = 0;
			agent[agtNum].repair = 0;
			agent[agtNum].name = agt_name;
			agent[agtNum].inconsistent = false;
			agent[agtNum].car = NULL;
			agtTable.insert({ agt_name, &agent[agtNum++] });
		}
		switch (cmd)
		{
		case 'p':
			cin >> car_name;
			_agt = agtTable.find(agt_name)->second;
			if (_agt->car != NULL) {
				_agt->inconsistent = true;
				continue;
			}

			// 차 등록
			_car = carTable.find(car_name)->second;
			_agt->car = _car;
			_agt->total += _car->rent;
			break;

		case 'r':
			cin >> distance;
			_agt = agtTable.find(agt_name)->second;
			if (_agt->car == NULL) {
				_agt->inconsistent = true;
				continue;
			}

			// 주행거리 계산 후 반납
			_car = _agt->car;
			_agt->total += (lli)_car->killo * distance;
			_agt->car = NULL;
			break;

		case 'a':
			cin >> broken;
			_agt = agtTable.find(agt_name)->second;
			if (_agt->car == NULL) {
				_agt->inconsistent = true;
				continue;
			}

			// 원가 * 사고율 계산
			_car = _agt->car;
			_agt->repair += ceil((double)_car->price * broken / 100);
			break;

		default:
			break;
		}
	}

	// 이름 순으로 정렬
	sort(agent, agent + agtNum);
}

void Print_Answer() {
	for (int i = 0; i < agtNum; i++) {
		if (agent[i].inconsistent || agent[i].car != NULL) cout << agent[i].name << " INCONSISTENT\n";
		else cout << agent[i].name << ' ' << agent[i].total + agent[i].repair << '\n';
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(nullptr); cout.tie(nullptr);

	int test_case;
	cin >> test_case;

	for (int t = 0; t < test_case; t++) {
		Input();
		Process();
		Print_Answer();
	}

	return 0;
}
```