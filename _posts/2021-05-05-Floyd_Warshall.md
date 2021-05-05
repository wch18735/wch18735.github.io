---
title: "[Algorithms] Floyd-Warshall"
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
  
플로이드-와샬 알고리즘은 두 가능한 노드의 모든 경로를 비교하여 계산합니다. 알고리즘을 간단하게 설명하기 위해 1부터 N까지 번호가 매겨진 노드로 구성된 그래프 **G**를 가정하겠습니다. 함수 **shortestPath(i, j, k)** 는 $i$ 에서$j$ 까지 이동하는데 집합 ${ 1, 2, ..., k}$ 내의 점을 거쳐 찾아오는 가장 짧은 경로 값을 반환합니다.  
  
각각의 (i, j) 쌍에 대하여 **shortestPath(i, j, k)** 는 다음과 같을 수 있습니다.
(1) 이전의 최소 경로에서 k를 거치지 않는 경우
(2) 이전의 최소 경로에서 k를 거치는 경우

(1)번은 k를 거치지 않으므로 **shortestPath(i, j, k-1)** 로 나타낼 수 있습다. (2)번은 k를 i에서 k까지의 최소 거리 값에 k에서 j로 가는 값의 합 **shortestPath(i, k, k-1)** + **shortestPath(k, j, k-1)** 이 됩니다. 이를 잘 표현한 식이 바로 아래 식입니다.  
{% highlight python %}
shortestPath(i, j, k) = min(shortestPath(i, j, k-1), shortestPath(i, k, k-1) + shortestPath(k, j, k-1))
{% endhighlight %}
이 공식이 플로이드-와샬 알고리즘의 핵심입니다. 알고리즘을 많이 풀어보신 분들이라면 이러한 점화식을 효율적으로 푸는데 많이 사용되는 Dynamic Programming을 떠올리실 수 있을 겁니다. 이 역시 k 값을 변화시키며 이전의 최소값을 저장해 계산하는 방식을 취합니다.

## Pseudocode
{% highlight python %}
let dist be a |V| X |V| array of minimum distances initialized to infinity
for each edge (u, v) do
    dist[u][v] ← w(u, v)  // The weight of the edge (u, v)
for each vertex v do
    dist[v][v] ← $ 0
for k from 1 to |V|
    for i from 1 to |V|
        for j from 1 to |V|
            if dist[i][j] > dist[i][k] + dist[k][j] 
                dist[i][j] ← dist[i]\[k] + dist[k][j]
            end if
{% endhighlight %}

## Negative cycle 에서 동작
만약 음의 순환 사이클이 존재한다면 어떻게 될까요? 위의 의사코드에서 확인할 수 있듯이 대각행렬 값은 모두 0으로 초기화 됩니다. 음수 순환 사이클이 존재한다면 어떠한 i에서 i로 돌아오는 대각행렬의 값이 음의 값을 취할 것입니다. 따라서 음수 순환 사이클 존재 여부를 확인하려면 대각행렬 값을 확인하면 알 수 있습니다.
  
## Path 추적
경로를 추적하는 것은 기존 코드에 약간의 수정을 통해 얻어낼 수 있습니다. 처음 (i, j)에 해당하는 next\[i]\[j] 경로에는 j를 추가해줍니다. 이후 기존 코드에서 (i, k) + (k, j) 경로가 더욱 작은 값을 가져 기존 매트릭스 값을 수정할 때 다음 노드 값을 함께 수정해줍니다.  
{% highlight python %}
let dist be a |V| X |V| array of minimum distances initialized to $\infty$
let next be a |V| X |V| array of vertex indices initialized to null

procedure FloydWarshallWithPathReconstruction() is
    for each edge (u, v) do
        dist[u][v] ← w(u, v)  // The weight of the edge (u, v)
        next[u][v] ← $ v
    for each vertex v do
        dist[v][v] ← $ 0
        next[v][v] ← $ v
    for k from 1 to |V| do // standard Floyd-Warshall implementation
        for i from 1 to |V|
            for j from 1 to |V|
                if dist[i][j] > dist[i][k] + dist[k][j] then
                    dist[i][j] ← dist[i][k] + dist[k][j]
                    next[i][j] ← next[i][k]
{% endhighlight %}

이렇게 저장된 행렬에서 (i, j) 에 해당하는 최소 경로를 반환하는 함수는 다음과 같이 구현할 수 있습니다.
{% highlight python %}
procedure Path(u, v)
    if next[u][v] = null then
        return [ ]
    path = [u]
    while u ≠ v
        u ← next[u][v]
        path.append(u)
    return path
{% endhighlight %}

## 소스코드
위의 개념을 활용하여 문제를 풀어볼 수 있습니다. 먼저 서로가 연결되어있는지를 확인하는 기본적인 문제 [백준 11403: 경로찾기](https://www.acmicpc.net/problem/11403)를 풀어보신 후 [백준 11404: 플로이드](https://www.acmicpc.net/problem/11404)를 풀어보시길 바랍니다. 아래는 두 문제의 Python 소스코드입니다.

### 백준 11403: 경로찾기
{% highlight python %}
N = int(input())

# 무한
INF = 2 << 31

# 인접 행렬
adj_matrix = [[] for i in range(N + 1)]
for i in range(1, N+1):
    adj_matrix[i] = [0] + list(map(int, input().split(' ')))


# isPath(i, j, k) -> if(adj_matrix[i][k] and adj_matrix[k][j]) 이면 존재
for k in range(1, N + 1):
    for i in range(1, N+1):
        for j in range(1, N+1):
            if adj_matrix[i][k] == 1 and adj_matrix[k][j] == 1:
                adj_matrix[i][j] = 1

for i in range(1, N+1):
    for j in range(1, N+1):
        print(adj_matrix[i][j], end=' ')
    print()
{% endhighlight %}

### 백준 11404: 플로이드
{% highlight python %}
N = int(input())
M = int(input())

# 갈 수 없음
INF = 2 << 31

# 인접 매트릭스
adj_matrix = [[INF if (i != j) else 0\
               for i in range(N+1)] \
              for j in range(N+1)]

# 인접 모두 더하기
for i in range(M):
    S, E, C = map(int, input().split(' '))
    if C < adj_matrix[S][E]:
        # 다른 도시로 갈 때 여러 루트가 존재하는 경우
        # 최소 비용만 추가
        adj_matrix[S][E] = C

def print_adj_matrix():
    for i in range(1, N+1):
        for j in range(1, N+1):
            if adj_matrix[i][j] == INF:
                print(0, end=' ')
            else:
                print(adj_matrix[i][j], end=' ')
        print()

def floyd_warshall():
    # 지나는 점 k, 시작점 i, 끝 점 j에 대하여
    # shortPath(i, j, k) = min( shortPath(i, j, k-1), shortPath(i, k, k-1) + shortPath(k, j, k-1) )
    # 로 나타낼 수 있다
    for k in range(1, N+1):
        for i in range(1, N+1):
            for j in range(1, N+1):
                if adj_matrix[i][j] > adj_matrix[i][k] + adj_matrix[k][j]:
                    adj_matrix[i][j] = adj_matrix[i][k] + adj_matrix[k][j]

    # print answer
    print_adj_matrix()

if __name__=="__main__":
    floyd_warshall()
{% endhighlight %}