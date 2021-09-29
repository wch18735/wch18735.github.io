---
title: "[Coding Test] 암호코드 스캔 (SWEA: 1244)"
excerpt: "brute force with dynamic programming memoization"
date: 2021-09-29
layout: single
classes: wide
category:
    - coding test
tag:
    - c++
    - software expert academy
    - brute force
    - dfs
    - dynamic programming
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-09-23/coding_test.jpg"
---

# 최대 상금 (SWEA: 1244)

[최대 상금 문제](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV15Khn6AN0CFAYD)는 주어지는 문자열의 자릿수를 교환해 최대 숫자를 만드는 문제입니다. 이때, 교환할 수 있는 횟수가 주어지며, 해당 숫자를 모두 소진해야 합니다.

## Brute Force

완전 탐색은 여러 방법으로 구현될 수 있습니다. 이번 문제에서는 DFS를 활용했습니다. 이 DFS 함수를 설계할 때, 한 가지 기억해두면 좋은 것이 있습니다.

**재귀함수는 귀납적 논증 과정**을 닮았다는 것입니다. 우리는 `내가 작성한 함수가 최대 값을 반환하는가?` 를 고민할 필요 없습니다. 간단히 설명하기 위해 아래와 같이 간단한 코드를 작성합니다.

```cpp
int dfs(int a, int b, int c){
    // 최대 값 반환
    if(a == b){
        return c
    }

    /*
    ... specific operation
    */

    int ret = dfs(a + 1, b, c);
    ret = (ret > c) ? ret : c;

    return ret;
}
```

`dfs()` 함수가 반환하는 값을 `ret` 변수에 저장합니다. 그런데 이 값과 우리가 비교하려는 값을 비교해 더 큰 값을 저장합니다. 즉, 이 과정에서 반환되는 값은 무조건 더 큰 값을 취하는 것을 보장할 수 있습니다.

따라서 **반환되는 값이 실제 최대 값인지?** 에 대한 고민은 필요 없으며, 반복적으로 발생하는 간단한 동작을 설계하는 것만으로도 구하려는 값이 최종적으로 반환됨을 확신할 수 있습니다. 

아래 소스코드에서도 `solve()` 함수를 통해 그 개념을 더 자세히 익혀보면 좋을 것 같습니다.

여기서 한 가지 팁이 있습니다. `int &ret = DP[cnt][number]`입니다. 바로 `ret` 변수가 `DP[cnt][number]`를 가리키도록 한 것입니다. C++ 에서만 사용 가능한 것으로 **Reference**라 불립니다. 

포인터와 다르게 레퍼런스에는 포인터 값을 한 번 밖에 할당 가능합니다. 새로운 포인터 재할당이 발생하면 에러를 반환합니다. 

그 밖에도 여러 차이점이 존재하는데, [이곳](https://www.geeksforgeeks.org/pointers-vs-references-cpp/)을 확인하면 더 자세히 알 수 있습니다.

중요하게 생각할 점은 `ret` 값에 반환값, 최대값 등이 할당되면 동시에 이 레퍼런스가 가리키고 있는 값도 함께 변화한다는 것입니다. 

코딩 테스트 중간중간 여러 곳에서 발생하는 **Memoization Cache** 변화를 추적할 필요 없습니다. 위와 같은 선언 한 번으로 아래에서 발생하는 모든 경우를 커버할 수 있습니다.

## 소스 코드

```cpp
#include<iostream>
#include<cstring>

using namespace std;

int parse_int(char*);
int length_of_str(char*);
int length_of_int(int);
int solve(int, int);
int swap_int(int, int, int);

int cache[11][1000000];

int main(int argc, char** argv) {
	//freopen("sample_input.txt", "r", stdin);

	int T, test_case;
	cin >> T;

	char numberString[7];
	int trial_cnt;

	for (test_case = 1; test_case <= T; test_case++) {
		cin >> numberString >> trial_cnt;

		memset(cache, -1, sizeof(cache));
		int number = parse_int(numberString);
		int answer = solve(parse_int(numberString), trial_cnt);

		cout << '#' << test_case << ' ' << answer << '\n';
	}

	return 0;
}

int solve(int number, int cnt) {
	if (cnt == 0)
		return number;

	if (cache[cnt][number] != -1)
		return cache[cnt][number];

	int length = length_of_int(number);
	int &ret = cache[cnt][number];
	int tmp = 0;

	for (int i = 0; i < length; i++) {
		for (int j = i + 1; j < length; j++) {
			tmp = solve(swap_int(number, i, j), cnt - 1);
			ret = (ret < tmp) ? tmp : ret;
		}
	}

	return ret;
}

int swap_int(int number, int left_idx, int right_idx) {
	int length = length_of_int(number);
	int* tmp_arr = new int[length];
	int digit = 0;

	int ret = number;
	for (int i = 0; i < length; i++) {
		tmp_arr[i] = ret % 10;
		ret /= 10;
	}

	if (tmp_arr[left_idx] == tmp_arr[right_idx]) {
		delete[] tmp_arr;
		return number;
	}

	digit = tmp_arr[left_idx];
	tmp_arr[left_idx] = tmp_arr[right_idx];
	tmp_arr[right_idx] = digit;

	ret = 0;
	for (int i = length - 1; i >= 0; i--)
		ret = 10 * ret + tmp_arr[i];

	return ret;
}

int parse_int(char* numberStr) {
	int idx = 0;
	int parsed = 0;
	while (*(numberStr + idx) != '\0') {
		parsed = parsed * 10 + *(numberStr + idx)-'0';
		idx++;
	}
	return parsed;
}

int length_of_str(char* numberString) {
	int idx = 0;
	while(*(numberString + idx++) != '\0'){}
	return idx - 1;
}

int length_of_int(int number) {
	if (number == 0) return 1;

	int cnt = 0;
	while (number != 0) {
		cnt++;
		number /= 10;
	}

	return cnt;
}
```