---
title: "[Algorithms] Bellman-Ford"
excerpt: "모든 점으로부터 연결된 모든 점까지의 최단 거리를 구하는 알고리즘"
date: 2021-05-05
layout: single
classes: wide
category:
    - algorithms
tag:
    - algorithm
    - floyd-warshall
toc: false
author: 1FeS
comments: true
---

# Floyd-Warshall Algorithm (플로이드-와샬 알고리즘)

플로이드-와샬 알고리즘은 직접 연결된 가중치를 가지는 그래프의 가장 짧은 거리를 찾는 알고리즘입니다. 이전의 다익스트라나 벨만-포드 알고리즘과는 다르게 모든 노드 i에 대해 노드 j로 가는 비용 d(i, j)의 최소 값을 구할 수 있습니다. 이때, **Negative Cycle** 이 존재하지 않는다면 양수나 음수 간선에 관계없이 최소 값을 알 수 있습니다. 비록 세부적인 **경로**를 보여주진 않으나 알고리즘 구조적으로 조금 수정하면 i에서 j로 가는 최소 경로를 반환할 수 있습니다.  
  
플로이드-와샬 알고리즘은 두 가능한 노드의 모든 경로를 비교하여 계산합니다. 알고리즘을 간단하게 설명하기 위해 1부터 N까지 번호가 매겨진 노드로 구성된 그래프 **G**를 가정하겠습니다. 함수 **shortestPath(i, j, k)** 는 $i$ 에서$j$ 까지 이동하는데 집합 $\left \{ 1, 2, ..., k \right \}$ 내의 점을 거쳐 찾아오는 가장 짧은 경로 값을 반환합니다.  
  
각각의 (i, j) 쌍에 대하여 **shortestPath(i, j, k)** 는 다음과 같을 수 있습니다.
(1) 이전의 최소 경로에서 k를 거치지 않는 경우
(2) 이전의 최소 경로에서 k를 거치는 경우

(1)번은 k를 거치지 않으므로 **shortestPath(i, j, k-1)** 로 나타낼 수 있습다. (2)번은 k를 i에서 k까지의 최소 거리 값에 k에서 j로 가는 값의 합 **shortestPath(i, k, k-1)** + **shortestPath(k, j, k-1)** 이 됩니다. 이를 잘 표현한 식이 바로 아래 식입니다.  
{% highlight %}
shortestPath(i, j, k) = min(shortestPath(i, j, k-1), shortestPath(i, k, k-1) + shortestPath(k, j, k-1))
{% endhighlight %}
이 공식이 플로이드-와샬 알고리즘의 핵심입니다. 알고리즘을 많이 풀어보신 분들이라면 이러한 점화식을 효율적으로 푸는데 많이 사용되는 Dynamic Programming을 떠올리실 수 있을 겁니다. 이 역시 k 값을 변화시키며 이전의 최소값을 저장해 계산하는 방식을 취합니다.