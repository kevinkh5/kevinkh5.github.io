---
title: '[Minimum Spanning Tree] 백준 1774번 우주신과의 교감 풀어보기'
author: baduk
date: 2024-02-02 16:44:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:<https://www.acmicpc.net/problem/1774>

## 문제 소개
황선자씨와 우주신들을 최소신장트리가 되도록 연결하는 문제다. 중요한 것은 미리 연결된 간선이 있어서 이부분을 빼고 신장트리로 연결해야한다.

## 문제 접근 및 풀이
문제에서 황선자씨와 우주신들을 최소거리로 연결한다는 부분을 보면 최소신장트리 문제인 것은 충분히 알 수 있을 것이다. 그럼 이제 단순히 최소신장트리를 구하면 될까? 이 문제의 핵심은 미리 연결된 간선이 있다는 부분이다. 무작정 모든 노드(우주신, 황선자씨)의 대해 최소신장트리를 구하면 안된다.

중요한 것은 미리 연결된 통로를 고려해서 최소신장트리를 만들어줘야 한다는 점인데, 나는 처음에 문제에서 ‘미리 연결된 통로’를 어떻게 고려할지 아이디어를 떠올리지 못했다.

이 부분에 대한 솔루션을 설명하자면, 미리 연결된 통로는 union해서 서로소 관계에 놓이도록 하여, 크루스칼 알고리즘으로 최소신장트리를 만들어가는 과정에서 새롭게 연결하는 것을 방지하고, 나머지 노드를 연결하여 최소신장트리를 구하면 된다.

처음에 이 아이디어를 떠올렸으면 풀 수 있었는데, 이 부분을 떠올리지 못했다.

## 제출코드
```python
def get_distance(a,b):
    a1 = (b[0] - a[0])**2
    b1 = (b[1] - a[1])**2
    return (a1+b1)**(1/2)


n,m = map(int,input().split())
my_edges = []
xy_lists = [0]*(n+1)
parent = [i for i in range(n+1)]
rank = [0]*(n+1)
already_linked = []

for i in range(1,n+1):
    x,y = map(int, input().split())
    xy_lists[i]=(x,y)

for i in range(1,n+1):
    for j in range(1,i):
        my_edges.append((get_distance(xy_lists[i],xy_lists[j]),i,j))

for i in range(m):
    x,y = map(int,input().split())
    already_linked.append((x,y))

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

def kruskal(edges):
    edges.sort()
    sum_weight = 0
    for w, a, b in edges:
        if find(a) != find(b):
            union(a,b)
            sum_weight += w
    return sum_weight

for x,y in already_linked:
    union(x,y)

# print(round(kruskal(my_edges),2))
number = kruskal(my_edges)
formatted_number = f"{number:.2f}" # 소수점 두자리 나타내기위한 포메팅
print(formatted_number)
```

## 후기
이 문제는 크루스칼 알고리즘을 잘 알고 쓸줄 아는지를 물어보는 문제인것 같다. 크루스칼은 안쓰다가 다시 쓸려니까 좀 까먹었다. 유니온, 파인드 함수까지는 그래도 기억이 나는데, 크루스칼 함수에서 처음에 파라미터 몇개를 받는지가 잘 생각이 안났고, 노드를 기준으로 받는지, 엣지를 기준으로 받는지 조금 가물가물했다. 크루스칼은 엣지로 받고, 그 엣지를 코스트를 기준으로 정렬한후 for문으로 노드 두개를 꺼내서 서로소관계인지 파악하면서 연결해 나가는 방식임을 잊지말자.