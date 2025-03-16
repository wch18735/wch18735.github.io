---
title: "[Coding Test] SortedList 구현"
excerpt: "sorted list implementation"
date: 2022-02-06
layout: single
classes: wide
category:
    - coding test
tag:
    - python
    - Beakjoon
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Sorted List 구현

이전 Hash 를 구현할 때와 비슷하게 parent, child Node 를 가지는 데이터 노드를 정의했다. Linked List 형태로 Root 로부터 child node 를 찾아가며 정확한 위치에 삽입하는 방식이다.

Linked List 의 단점으로는 특정 Index 값을 알아보기까지 계속해서 값을 찾아나가야 하는 단점이 있다. 반대로 삽입은 link 값만을 수정하면 되기 때문에 빠르다는 장점이 있다.

```cpp
struct Node {
	int data;
	Node* parent;
	Node* child;

	void insert(int data) {
		this->data = data;
	}

	void init() {
		data = 0;
		parent = nullptr;
		child = nullptr;
	}
};

struct SortedList {
	Node node[100'000];
	Node* root;
	int nodeNum;

	void init() {
		root = nullptr;
		for (int i = 0; i < 100'000; i++)
			node[i].init();
	}

	void insert(int data) {
		Node* i = &node[nodeNum++];
		i->data = data;

		// 초기 데이터
		if (root == nullptr) {
			root = i;
			return;
		}

		Node* cur = root;
		while (true) {
			// 입력 데이터 < 현재 데이터
			if (i->data < cur->data) {
				Node* parent = cur->parent;
				
				// root
				if (parent == nullptr) {
					root = i;
					i->child = cur;
					cur->parent = i;
					return;
				}

				// parent node link
				parent->child = i;
				i->parent = parent;

				// child node link
				i->child = cur;
				cur->parent = i;
				
				// 삽입 완료
				return;
			}

			// 마지막 노드
			if (cur->child == nullptr) {
				cur->child = i;
				i->parent = cur;
				i->child = nullptr;
				return;
			}

			// 다음 노드로 이동
			cur = cur->child;
		}
	}

	int remove(int index) {
		int idx = 0;
		
		if (index == 0) {
			int data = root->data;
			root = root->child;
			root->parent = nullptr;
			return data;
		}

		Node* target = root;
		while (idx != index) {
			target = target->child;
			idx++;
		}

		int data = target->data;
		if (target->child == nullptr) {
			target->parent->child = nullptr;
			return data;
		}

		Node* parent = target->parent;
		Node* child = target->child;
		parent->child = child;
		child->parent = parent;
		return data;
	}

	int searchByIndex(int index) {
		int idx = 0;
		Node* target = root;

		while (idx++ != index)
			target = target->child;

		return target->data;
	}
} SL;
```