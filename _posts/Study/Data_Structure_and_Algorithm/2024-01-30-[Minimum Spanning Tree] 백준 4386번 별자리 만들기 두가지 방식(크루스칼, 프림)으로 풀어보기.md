---
title: '[Minimum Spanning Tree] 백준 4386번 별자리 만들기 두가지 방식(크루스칼, 프림)으로 풀어보기'
author: baduk
date: 2024-01-30 17:36:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
## 문제소개
각 별자리의 좌표(x,y)가 주어진다. 이 별자리들을 일직선으로 이었을 때 최소비용구하라.

## 문제접근
문제를 읽고 별자리를 직접 한번 그려보면 최소신장트리를 구하는 문제라는 것을 알 수 있다. 하지만 이 문제는 다른 쉬운 최소신장트리를 구하는 문제와는 달리, 노드를 입력으로 따로 주지 않고 간선의 비용역시 따로 주지 않는다.

따라서 이 문제에서 시간을 들여야할 부분이 바로 이 `데이터를 어떻게 받아서` 최소신장트리의 비용을 구할 것 인가에 대한 부분이다.

이 문제는 두가지 알고리즘으로 풀 수 있다. 첫번째는 크루스칼 알고리즘을 사용하는 방법이고, 두번째는 프림 알고리즘을 사용하는 방법이다.

둘중 어느 알고리즘을 쓰든 둘다 시간복잡도는 O(E*log(E))로 큰 차이는 없을 것 같다.

## 크루스칼 알고리즘 간단 요약
크루스칼 알고리즘은 입력을 간선 데이터를 기준으로 받는다. 그리고 간선의 코스트를 오름차순으로 정렬하여 코스트가 작은 간선부터 하나씩 빼나가면서 진행하는데, 먼저 그 간선을 잇고 있는 두 노드가 서로소 집합인지 확인한다.

두 노드가 서로소 집합이면 연결(union)해주고 간선의 코스트를 업데이트한다. 만약 두 노드가 서로소 집합이 아니면(즉, 동일한 루트 노드를 가지고 있다면) 연결해 주었을 때 싸이클을 형성하게 될 수 있다. 따라서 이때는 코스트 업데이트 하지 않고 by pass하도록 진행한다. 

## 프림 알고리즘 간단 요약
프림 알고리즘은 크루스칼 알고리즘과 달리 입력을 노드 데이터를 기준으로 받는다. 이러한 입력에 대해서 노드로 주소를 접근하여 간선의 코스트와 그 노드에 연결된 다른 노드에 대한 데이터를 가져올 수 있다.

프림 알고리즘은 노드를 가지고 최소신장트리를 찾아나간다. 처음에 시작할 노드를 주소로 하여 그 노드에 연결된 간선데이터를 최소힙에 넣고 코스트가 작은 간선과 그 간선을 연결하고 있는 노드 꺼내면서, 코스트를 업데이트 해나간다. 그리고 이 과정에서 꺼낸 노드를 주소로하여 그 노드와 연결된 간선데이터를 최소힙에 넣어준다. 이 때 이미 꺼낸적 있는 노드는 싸이클 형성 방지를 위해 by pass한다.

## 크루스칼 알고리즘 vs 프림 알고리즘

```
크루스칼 알고리즘
입력 데이터 : 간선기준
사용 변수 : rank, parent, edge
추가 사용 알고리즘 : 정렬(sort), 서로소집합(disjoint set)
시간복잡도 : Elog(E)
```

```
프림 알고리즘
입력 데이터 : 노드기준
사용 변수 : adj_edges, visited
추가 사용 알고리즘 : 최소힙(heap)
시간복잡도 : Elog(E) (개선된 프림 알고리즘의 경우 Elog(V), 여기서는 Elog(E))
```

둘다 시간복잡도가 같기 때문에, 어떤것을 쓰든 크게 상관없을 것 같다. 개인적으로 느끼기에 입력받을 데이터를 만들어 줄 때, 프림 알고리즘이 조금 더 무거운 느낌이 있다. 프림함수에서 입력받을 초기 데이터는 노드 기준인데, 노드 안에 연결된 여러 간선들에 대한 데이터를 삽입해야하다보니, for문이 도는 과정에서 append를 두번씩 하게 되기 때문이다.

실제로 문제 채점 결과 메모리와 시간 모두 프림 알고리즘을 사용했을 때가 크루스칼 알고리즘을 사용했을 때 보다 더 높게 나왔다.(물론 큰 차이는 없었지만)

하지만 여러 초기변수들을 설정해서 진행하는 크루스칼 알고리즘 보다 프림 알고리즘이 코드가 좀 더 간결해보이긴한다.

## 크루스칼 알고리즘으로 푼 코드
```python
# 크루스칼 알고리즘
n = int(input())
rank = [0]*n
parent = [0]*n
node_xy = [tuple(map(float, input().split())) for _ in range(n)]

def find(node):
    if node != parent[node]:
        parent[node] = find(parent[node])
    return parent[node]

def union(a,b):
    root1 = find(a)
    root2 = find(b)

    if rank[root1] > rank[root2]:
        parent[root2] = root1
    else:
        parent[root1] = root2
        if rank[root1] == rank[root2]:
            rank[root2] += 1

def kruskal(e):
    e.sort()
    sum_w = 0
    for w, n1, n2 in e:
        if find(n1) != find(n2):
            union(n1,n2)
            sum_w += w
    return sum_w

edges = []
for i in range(n):
    parent[i] = i
    for j in range(i):
        a = (node_xy[i][0] - node_xy[j][0])**2
        b = (node_xy[i][1] - node_xy[j][1])**2
        w = (a + b)**(1/2)
        edges.append((w,i,j))

print(round(kruskal(edges),2))
```

## 프림 알고리즘으로 푼 코드
```python
# 프림 알고리즘
import heapq
n = int(input())
adj_edges = [[] for _ in range(n)]
node_xy = [tuple(map(float,input().split())) for i in range(n)]

for i in range(n):
    for j in range(i):
        a = (node_xy[i][0] - node_xy[j][0])**2
        b = (node_xy[i][1] - node_xy[j][1])**2
        w = (a+b)**(1/2)
        adj_edges[j].append((w, i))
        adj_edges[i].append((w, j))

visitied = [0]*n

def prim(start, edges):
    pq = edges[start]
    heapq.heapify(pq)
    visitied[start] = 1
    sum_cost = 0
    while pq:
        cost, node = heapq.heappop(pq)
        if not(visitied[node]):
            visitied[node] = 1
            sum_cost += cost
            for c, n in edges[node]:
                if not(visitied[n]):
                    heapq.heappush(pq, (c, n))
    return sum_cost

print(round(prim(0, adj_edges),2))
```