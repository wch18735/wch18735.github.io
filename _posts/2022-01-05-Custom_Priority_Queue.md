---
title: "[Coding Test] 값 변화를 즉각 반영하는 우선순위 큐"
excerpt: "custom priority queue"
date: 2022-01-05
layout: single
classes: wide
category:
    - coding test
tag:
    - cpp
    - priority queue
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
header:
    teaser: "/_img/2021-09-23/coding_test.jpg"
---

이번 문제는 주어진 값이 바뀌었을 때, 값 변화를 인지하고 최대 또는 최소 값을 뽑아내는 우선순위 큐를 구현하는 [수열과 쿼리 15](https://www.acmicpc.net/problem/14427)이다. 아래는 풀이 방법이다.

```cpp
#include<iostream>

using namespace std;

#define maxnum 100'005

struct Point {
	int index, value, heap_index;
} sequence[maxnum];

struct Heap {
	// target index to fill (== empty)
	int N = 0;
	
	Point *data[maxnum];

	bool compare(int heap_index_a, int heap_index_b)
	{
		int cmp = data[heap_index_a]->value - data[heap_index_b]->value;
		if (cmp == 0)
			cmp = data[heap_index_a]->index - data[heap_index_b]->index;
		return cmp < 0;
	}

	void swap(int heap_index_a, int heap_index_b)
	{
		// 외부 데이터의 heap 에서 index 값 변경
		data[heap_index_a]->heap_index = heap_index_b;
		data[heap_index_b]->heap_index = heap_index_a;

		// heap 내부 포인터 변경
		Point *tmp = data[heap_index_a];
		data[heap_index_a] = data[heap_index_b];
		data[heap_index_b] = tmp;
	}

	int update_down(int heap_index)
	{
		while (true) {
			int child_left = heap_index * 2 + 1;
			int child_right = heap_index * 2 + 2;

			if (child_left >= N)
				break;

			// 최종 비교 index
			int child = child_left;

			if (child_right < N)
				child = compare(child_left, child_right) ? child_left : child_right;

			if (compare(heap_index, child))
				break;

			swap(heap_index, child);
			heap_index = child;
		}
		return heap_index;
	}

	int update_up(int heap_index)
	{
		while (true)
		{
			if (heap_index == 0)
				break;

			int parent = (heap_index - 1) / 2;

			if (compare(parent, heap_index))
				break;

			swap(parent, heap_index);
			heap_index = parent;
		}

		return heap_index;
	}

	void update(int heap_index)
	{
		int index = update_down(heap_index);
		update_up(heap_index);
	}

	void push(Point *p) {
		data[N] = p;
		p->heap_index = N;
		N++;
		update(p->heap_index);
	}

	void pop() {
		swap(0, N - 1);
		N--;
		update(0);
	}

	int top() {
		if (N == 0) return -1;
		return data[0]->index;
	}

	void clear() {
		N = 0;
	}
};

int main(int argc, char* argv[])
{
	//freopen("sample_input.txt", "r", stdin);
	ios_base::sync_with_stdio(0);
	cin.tie(0); cout.tie(0);

	int N;
	cin >> N;
	
	Heap heap;
	for (int i = 1; i <= N; i++)
	{
		int a;
		cin >> a;

		sequence[i].index = i;
		sequence[i].value = a;
		heap.push(&sequence[i]);
	}

	int M;
	cin >> M;

	for (int i = 0; i < M; i++)
	{
		int query;
		cin >> query;

		if (query == 2)
			cout << heap.top() << '\n';
		else if (query == 1)
		{
			int i, v;
			cin >> i >> v;
			sequence[i].value = v;
			heap.update(sequence[i].heap_index);
		}
	}

	return 0;
}
```