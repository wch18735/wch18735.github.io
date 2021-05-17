---
title: "[Coding Test] lower bound and upper bound"
excerpt: "how to use lower bound and upper bound concept"
date: 2021-05-16
layout: single
classes: wide
category:
    - coding test
tag:
    - Python
    - Dynamic Programming
    - Baekjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Lower Bound and Upper Bound

문제에서 Lower bound, Upper bound를 직접 구현해서 풀었을 때는 **Time Out**을 받았으나, 주어진 함수를 사용했더니 정답을 받았습니다. 아래는 구현 예시이며, 설명은 사용할 때 헷갈리는 점을 정리해 놓은 것입니다. 이는 오름차순을 기준으로 합니다.

- `lower_bound` 는 **target** 값이 여러 개일 때, 가장 왼쪽 주소값을 반환
- `upper_bound` 는 **target** 값이 여러 개일 때, 가장 오른쪽 주소값을 반환
- `lower_bound`와 `upper_bound` 모두 **target** 값이 없을 때, **target**보다 큰 가장 작은 정수의 주소값을 반환

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main(int argc, char* argv[])
{
	// example array
	int arr1[8] = { 1, 2, 3, 4, 4, 4, 5, 6 };
	int arr2[8] = { 1, 2, 3, 3, 5, 5, 6, 7 };
	
	cout << "arr1's elements: ";
	for (int i = 0; i < 8; i++) cout << arr1[i] << ' ';
	cout << '\n';
	cout << "arr2's elements: ";
	for (int i = 0; i < 8; i++) cout << arr2[i] << ' ';
	cout << '\n';

	// test arrays
	vector<int> test_arr1(arr1, arr1 + 8);
	vector<int> test_arr2(arr2, arr2 + 8);

	cout << "array1 test of lower_bound and upper_bound" << '\n';
	cout << lower_bound(test_arr1.begin(), test_arr1.end(), 4) - test_arr1.begin() << '\n';
	cout << upper_bound(test_arr1.begin(), test_arr1.end(), 4) - test_arr1.begin() << '\n';
	cout << "array2 test of lower_bound and upper_bound" << '\n';
	cout << lower_bound(test_arr2.begin(), test_arr2.end(), 4) - test_arr2.begin() << '\n';
	cout << upper_bound(test_arr2.begin(), test_arr2.end(), 4) - test_arr2.begin() << '\n';

	return 0;
}
```

```console
arr1's elements: 1 2 3 4 4 4 5 6
arr2's elements: 1 2 3 3 5 5 6 7
array1 test of lower_bound and upper_bound
3
6
array2 test of lower_bound and upper_bound
4
4
```