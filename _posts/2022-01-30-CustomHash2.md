---
title: "[Coding Test] 삭제가 가능한 Hash 구현"
excerpt: "implementation hash"
date: 2022-01-30
layout: single
classes: wide
category:
    - coding test
tag:
    - hash
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

이전 함수에서 `remove()` 함수를 추가했다. Double Linked List 형태로 노드를 구현해 삭제 시 용이하게 만들었다.

```cpp
#define TableSize 50000
#define MaxKeySize 11

struct Node {
	int data;
	char key[MaxKeySize];
	Node* child;
	Node* parent;
};

void mstrcpy(char* dest, const char* src) {
	int idx = 0;
	while ((dest[idx] = src[idx]) != '\0') ++idx;
}

int mstrcmp(char* str1, char* str2) {
	int idx = 0;
	while (str1[idx] != '\0' && str1[idx] == str2[idx]) ++idx;
	return str1[idx] == str2[idx] ? true : false;
}

struct Hash {
	int hashNum;
	Node node[100'005];
	Node* hashTable[TableSize];

	void init() {
		hashNum = 0;
		for (int i = 0; i < 100'005; i++) {
			node[i].child = nullptr;
			node[i].parent = nullptr;
		}
		for (int i = 0; i < TableSize; i++)
			hashTable[i] = nullptr;
	}

	unsigned long getHash(char* str) {
		unsigned long hash = 5381;
		int c;
		while (c = *str++)
			hash = (hash << 5) + hash + c;
		return hash % TableSize;
	}

	void add(char* key, int data) {
		Node* n = &node[hashNum++];
		unsigned long hKey = getHash(key);

		// Key, Value 삽입
		mstrcpy(n->key, key);
		n->data = data;

		// 충돌 조치
		Node* prev = hashTable[hKey];
		n->child = prev;
		if(prev != nullptr)
			prev->parent = n;
		hashTable[hKey] = n;
	}

	int get(char* key) {
		unsigned long hKey = getHash(key);
		for (Node* n = hashTable[hKey]; n != nullptr; n = n->child) {
			// Key 값 일치하면 반환
			if (mstrcmp(n->key, key)) return n->data;
		}
		return -1;
	}

	int remove(char* key) {
		unsigned long hKey = getHash(key);
		int data = -1;
		for (Node* n = hashTable[hKey]; n != nullptr; n = n->child) {
			// Key 값 일치하지 않으면 Pass
			if (!mstrcmp(n->key, key)) continue;

			// 삭제하는 데이터
			data = n->data;
			
			if (n->parent == nullptr) {
				// 삭제하는 데이터의 parent 노드가 존재하지 않는 경우
				hashTable[hKey] = n->child;
				if (n->child != nullptr) n->child->parent = nullptr;
				break;
			}
			else {
				// 삭제하는 데이터의 parent 노드가 존재하는 경우
				n->parent->child = n->child;
				if (n->child != nullptr) {
					n->child->parent = n->parent;
				}
				break;
			}
		}
		return data;
	}
};

Hash cHash;
```