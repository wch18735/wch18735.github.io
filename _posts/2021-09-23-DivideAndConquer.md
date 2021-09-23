---
title: "[Coding Test] divide and conquer"
excerpt: "divide and conquer in cpp"
date: 2021-09-23
layout: single
classes: wide
category:
    - coding test
tag:
    - cpp
    - divide and conquer 
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-09-23/divide_and_conquer.jpg"
---

# Divide and Conquer

분할 정복은 하나의 문제를 잘게 쪼개어 문제를 해결하는 방식입니다. 그 대표적인 예시로는 **병합정렬(Merge Sort)** 이 있습니다. 모든 경우에 $N \space log{N}$ 의 복잡도를 가집니다. 아래는 직접 작성해본 merge sort 소스코드 입니다.

```cpp
#include<stdio.h>

void merge_sort(int* arr, int n)
{
    // 경계조건
	if (n == 1) return;

    // 길이가 n인 정수형 임시 배열
	int* temp = new int[n];

    // 분할지점 mid
	int mid = n / 2;

    // 분할정복 재귀
	merge_sort(arr, mid);
	merge_sort(arr + mid, n - mid);

	int leftside = 0;
	int rightside = mid;
	int temp_idx = 0;

    // 병합 과정
	while (leftside < mid && rightside < n)
	{
		if (arr[leftside] < arr[rightside])
		{
			temp[temp_idx] = arr[leftside];
			leftside += 1;
		}
		else
		{
			temp[temp_idx] = arr[rightside];
			rightside += 1;
		}
		temp_idx += 1;
	}

    // 남은 부분 병합
	while (leftside < mid)
	{
		temp[temp_idx] = arr[leftside];
		leftside += 1;
		temp_idx += 1;
	}

	while (rightside < n)
	{
		temp[temp_idx] = arr[rightside];
		rightside += 1;
		temp_idx += 1;
	}

    // 병합된 배열 원래 배열에 삽입
	for (int i = 0; i < n; i++) arr[i] = temp[i];
	
    // 메모리 해제 (new <-> delete)
	delete temp;
}

int main(int argc, char** argv)
{
	int arr[9] = { 4, 3, 5, 1, 2, 8, 7, 9, 6 };
	merge_sort(arr, 9);

	for (int i = 0; i < sizeof(arr) / sizeof(int); i++) printf("%d ", arr[i]);

	return 0;
}
```

이 `merge_sort.cpp` 를 실행해보면 아래와 같은 결과를 얻을 수 있습니다.

```
result: 1 2 3 4 5 6 7 8 9
```

이때, argument로 포인터가 전달됩니다. 전달되는 부분은 전체 배열의 일부로 재귀 함수 내에서 **index 0** 으로 접근해도 **전체 배열에서 접근되는 위치는 다른 부분**을 가리킵니다.