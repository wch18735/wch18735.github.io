---
title: "[Algorithms] Trie"
excerpt: "한 점으로부터 연결된 모든 점까지의 최단 거리를 구하는 알고리즘"
date: 2022-08-18
layout: single
classes: wide
category:
    - algorithm
tag:
    - algorithm
    - trie
toc: false
author: 1FeS
comments: true
---

# Trie

Trie 란 Prefix Tree 라고도 불리는, 문자열을 문자 단위로 분해하여 Tree 구조를 만들어 검색을 빠르게 만드는 탐색트리의 일종인 자료구조이다. 문자열 검색 문제를 풀 때는 Hash Table 고 함께 자주 쓰이지만, 아마 익숙하지 않은 사람들이 많을 것이라 생각한다. 한 학기에 걸쳐 배우는 자료구조 및 알고리즘에선 기껏해야 막바지에 배우게 되는 자료구조이기 때문이다. 

길이가 M 이하인 서로 다른 문자열 N 개가 알파벳 순서로 정렬되어 있다고 가정하자. 이 때, 주어진 문자열 S가 N 개의 문자열과 일치하거나 그 부분 집합인지를 판단하려고 한다. 가장 먼저 떠오르는 방법은 이중 for 문으로 모든 문자열을 순회하며 대조해보는 것. 이는 $O(N^2)$ 시간 복잡도를 가진다. 두 번째로는 정렬되어있기 때문에 Binary Search 를 사용하는 방법도 가능하다. 이는 조금은 개선된 $O(M logN)$ 의 시간 복잡도를 가진다. 하지만 Trie 자료구조를 사용하면 $O(M)$ 시간 복잡도로 검색이 가능하다.

문자열은 정수나 실수와는 달리 길이가 변하기 때문에 검색에 시간이 많이 소요된다. 이러한 문제점을 해결하기 위해 고안된 자료구조가 바로 Trie 로 문자열을 다루는 많은 문제에서 제한시간 안에 문자열 검색을 수행해야 할 때는 이 자료구조를 기억해 놓았다가 적절하게 응용해보자.

## Trie 자료구조 형태

Trie 자료구조는 다음 규칙을 따라 구성된다.

1. 루트는 빈 노드를 가진다
2. 부모 노드에 접미 문자 하나씩을 붙여서 자식 노드를 생성한다
3. 간선은 접미 문자가 붙는 것을 나타낸다
4. 종료 노드를 두어 문자열의 끝을 알리도록 한다

위 규칙을 따라 문자열 집합 S = {"bear", "bell", "bid", "bull", "buy", "sell", "stock", "stop"} 로 Trie 자료구조를 구현하면 아래 그림과 같이 나타낼 수 있다.

<img src="/_img/2022-08-19/trie_structure.png">

## Trie 자료구조 구현

예제 난이도를 쉽게하기 위해 문자열은 알파벳 소문자로만 이루어진다고 가정하자. 그러면 Trie 구조체는 아래와 같이 구현할 수 있다.

```cpp
#include<iostream>

using namespace std;

#define MAXLETTER 26
#define CHAR_TO_INDEX(ch) ((int)ch - (int)'a')

typedef struct trie_node {
	int value;
	trie_node* children[MAXLETTER];

	void init() {
		value = 0;
		for (int i = 0; i < MAXLETTER; i++) children[i] = NULL;
	}

	trie_node* getNode() {
		trie_node* pNode = NULL;
		pNode = (trie_node*)malloc(sizeof(trie_node));

		if (pNode) {
			pNode->value = 0;
			for (int i = 0; i < MAXLETTER; i++) pNode->children[i] = NULL;
		}
		return pNode;
	}

	void insert(char str[]) {
		trie_node* pNode = this;
		int max_idx = 0;
		while (str[max_idx] != '\0') max_idx++;

		for (int i = 0; i < max_idx; i++) {
			// move
			int idx = CHAR_TO_INDEX(str[i]);
			if (pNode->children[idx]) {
				pNode = pNode->children[idx];
				continue;
			}

			// create trie node
			trie_node* newTrieNode = getNode();

			// insert and move
			pNode->children[idx] = newTrieNode;
			pNode = pNode->children[idx];
		}

		pNode->value += 1;
	}

	int search(char str[]) {
		trie_node* pNode = this;
		int max_idx = 0;
		while (str[max_idx] != '\0') max_idx++;

		for (int i = 0; i < max_idx; i++) {
			int idx = CHAR_TO_INDEX(str[i]);
			if (!pNode->children[idx]) return 0;

			pNode = pNode->children[idx];
		}

		return pNode->value;
	}
};

char inputStr[20];
char targetStr[20];

int main(int argc, char* argv[]) {
	trie_node TRIE;
	TRIE.init();

	for (int i = 0; i < 5; i++) {
		cin >> inputStr;
		TRIE.insert(inputStr);
	}

	cin >> targetStr;
	cout << TRIE.search(targetStr);

	return 0;
}
```

위 코드는 문자열 5개를 insert 하고 검색해 볼 수 있다. 다른 코드들을 보면 Trie 자료 구조의 root 와 node 를 따로 구현한 것도 있고, 구조체화 시키지 않고 함수들이 바깥에 정의된 코드도 있다. 핵심은 `어떻게 추가`, `어떻게 탐색` 하는지를 체화하는 것 같다.

추가와 탐색이 익숙해지면 다양한 변화를 주어 용도에 맞게 알고리즘을 수정할 수 있다. 가령, 주어진 문자열이 기존 문자열들의 부분집합 중 하나에 속하는지 확인하려면 탐색 경로에 NULL 값이 있는지만 확인하면 된다.

## Trie 관련 문제

Trie 관련 문제들을 풀어보며 본인만의 개념을 익히는 것이 중요하다. 생성 및 추가 관련해 자꾸 에러가 난다면 Linked List 를 이용해 문제 풀이부터 진행해보는 것도 좋을 것 같다.