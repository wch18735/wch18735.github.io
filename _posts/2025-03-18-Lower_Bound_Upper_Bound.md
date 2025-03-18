---
title: "[Coding Test] Lower Bound and Upper Bound"
excerpt: "how to use lower bound and upper bound concept"
date: 2025-03-18
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

# Lower Bound & Upper Bound

이분 탐색을 배웠다면, 이제 그 응용을 공부하고 문제를 풀어 볼 차례.

## Lower Bound와 Upper Bound란?

정렬된 배열에서 원하는 값을 **빠르게 찾는 방법**이 필요할 때, **Lower Bound**와 **Upper Bound** 개념을 활용할 수 있다.  
이 둘은 **이진 탐색(Binary Search)** 을 기반으로 하며, 특정 값의 **위치**나 **범위**를 찾는 데 사용된다.

## 언제 사용될까?

1. **어떤 값이 배열에서 몇 개 존재하는지 알고 싶을 때**
2. **주어진 값 이상의 가장 작은 위치를 찾을 때**
3. **주어진 값보다 큰 첫 번째 위치를 찾을 때**

이 개념을 알면 단순한 **이진 탐색(Binary Search)** 을 확장하여 다양한 문제를 해결할 수 있다.

## Lower Bound vs Upper Bound

| 개념        | 동작 방식 |
|------------|--------------------------------------|
| **Lower Bound** | 특정 값 이상의 **첫 번째 위치** 찾기 |
| **Upper Bound** | 특정 값 초과의 **첫 번째 위치** 찾기 |

**둘 다 존재하지 않는 값이라면?**  
→ `Lower Bound`와 `Upper Bound`는 모두 **해당 값보다 큰 값이 처음 등장하는 위치**를 반환한다.  
→ 배열의 끝을 넘어가면, 찾고자 하는 값이 **배열에 존재하지 않는다**는 의미가 된다.

## 예제 상황

예를 들어, 아래와 같은 배열이 있다고 하자.

```python
arr = [1, 2, 4, 4, 4, 5, 6]
```

- `lower_bound(4)` → **4가 처음 등장하는 위치** → `index 2`
- `upper_bound(4)` → **4보다 큰 값(5)이 처음 등장하는 위치** → `index 5`

## 코드 구현

내가 lower_bound, upper_bound 를 구현할 때 기억하는 3가지가 있다.

- lower_bound, upper_bound 구현은 딱 한 군데만 차이난다
- lower_bound 는 특정 수 이상인 첫 위치를 찾는다
- upper_bound 는 특정 수 초과인 첫 위치를 찾는다

```python
def lower_bound(arr: list, target: int):
	left, right, mid = 0, len(arr), 0

	while left < right:
		mid = (left + right) // 2

		if arr[mid] < target: # 여기 부등호만 다르다
			left = mid + 1
		else:
			right = mid
	
	return left

def upper_bound(arr: list, target: int):
	left, right, mid = 0, len(arr), 0
	
	while left < right:
		mid = (left + right) // 2

		if arr[mid] <= target: # 여기 부등호만 다르다
			left = mid + 1
		else:
			right = mid

	return left
```

따지고 보면 굉장히 간단하다.

lower_bound 함수는 주어진 target 값 이상인 곳을 찾는 함수다. 그래서 target 값보다 작은 것이 발견되는 순간 그 다음 위치를 정답으로 저장한다. 마찬가지로, upper_bound 함수는 target 값 초과인 곳을 찾는 함수다. 그래서 target 값보다 같거나 작은 곳이 발견되면 그 다음 위치를 정답으로 저장한다.

이제 아래 문제들을 풀어보자.

- [Leetcode 문제](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)
- [백준 문제](https://www.acmicpc.net/problem/1572)

여러 문제들을 접할 수 있을텐데, 각자가 경험한 주의해야 할 점을 댓글에 남겨주면 다른 사람에게도 큰 도움이 될 것 같다!