---
title: "[Coding Test] Hash 구현"
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

# Hash 구현

`char*` 타입을 키, `int` 타입을 값으로 하는 해쉬를 구현하려 한다. STL을 사용할 수 있다면 `unordred_map<string, int>` 이렇게 쉽게 만들 수 있다. 만약 사용이 불가능하다면? 아래와 같이 구현할 수 있다. 테이블 사이즈는 10,000 이며 입력된느 문자열의 길이는 10이 최대라 하자.

```cpp
#define TableSize 10000
#define MaxkeySize 11

struct Node {
	int data;
	char key[MaxkeySize];

	// Key 충돌 고려
	Node* prev;
};

void mstrcpy(char* dest, const char* src) {
	int idx = 0;
	while ((dest[idx] = src[idx]) != '\0') ++idx;
}

int mstrcmp(char* str1, char* str2) {
	int idx = 0;
	while (str1[idx] != '\0' && str1[idx] == str2[idx]) ++idx;
	return str1[idx] - str2[idx];
}

struct Hash {
	int nodeNum;
	Node node[100'000];
	Node* hashTable[TableSize];

	void init() {
		nodeNum = 0;
		for (int i = 0; i < 100'000; i++)
			node[i].prev = 0;
		for (int i = 0; i < TableSize; i++)
			hashTable[i] = 0;
	}

	Node* getNode() {
		return &node[nodeNum++];
	}

	unsigned long getHash(char* str) {
		unsigned long hash = 5381;
		int c;
		while (c = *str++) {
			hash = (((hash << 5) + hash) + c) % TableSize;
		}
		return hash % TableSize;
	}

	void add(char* key, int data) {
		int hashKey = getHash(key);
		Node* n = getNode();

		/* Data, Key 값 복사 */
		n->data = data;
		mstrcpy(n->key, key);

		/* 해당 hashKey 값에 충돌 발생시 prev 에 저장 */
		n->prev = hashTable[hashKey];
		hashTable[hashKey] = n;
	}

	int get(char* key) {
		int hashKey = getHash(key);

		for (Node* n = hashTable[hashKey]; n != 0; n = n->prev) {
			if (!mstrcmp(n->key, key)) return n->data;
		}
		return -1;
	}
};
```

구현을 위해 숙지해야 될 사항은 아래와 같다.

- mstrcpy
- mstrcmp
- hash 반환 함수
```cpp
unsigned long getHash(char* str) {
		unsigned long hash = 5381;
		int c;
		while (c = *str++) {
			hash = (((hash << 5) + hash) + c) % TableSize;
		}
		return hash % TableSize;
	}
```
- Node* prev 를 통한 충돌 조치