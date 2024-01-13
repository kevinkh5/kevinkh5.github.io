---
title: '[Heap + Topology Sort] 백준 1766번 문제집 힙과 위상정렬을 활용하여 풀기'
author: baduk
date: 2024-01-13 08:18:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Heap, 힙, python, problem solving, algorithm]
use_math: true
---

## 백준 1766번 문제집
이 문제는 위상정렬 알고리즘을 모르면 진짜 똑똑한 천재가 아닌 이상 풀 수 없을 것 같다. 일단 위상정렬이 무엇인지 부터 알아보자

## 위상정렬(Topology Sort)?
위키 백과에 있는 위상정렬의 정의를 보면 `"위상 정렬(topological sorting)은 유향 그래프의 꼭짓점들(vertex)을 변의 방향을 거스르지 않도록 나열하는 것을 의미"` 라고 정의한다. 좀 더 이해하기 쉽게 말하자면 반드시 지켜져야 하는 순서를 반영하면서 요소들을 정렬하는 알고리즘이다.

예를 들어서 아래와 같이 1번이 3번보다 먼저와야 하고 3번이 2번보다 먼저와야하며, 4번이 3번보다 먼저 와야한다고 해보자.
```
1 -> 3
3 -> 2
4 -> 3
```


위 표시를 그림(Graph)으로 표현해보자면 아래와 같다.


<img src='https://lh3.googleusercontent.com/pw/ABLVV85Jd9iB7JQzAebgvbMtWEYHacZAp5fqvMuAFFHhgmYObclo0DdbyR-pKU2BGQDfBx-tfxgrw3Eo66zcz8RRgy0UzUID-N-MUOHor2uycIvvi0LNb0vi0su79sghuSkiaOvkjdLlt2YyI_QYN5aNOOYe=w1472-h831-s-no-gm?authuser=0' alt='exam' height=400>

위상정렬이 돌아가는 방식을 간단하게 설명해보겠다.

먼저 위 그림에서 진입차수(가리킴을 받는 화살표의 개수)가 0인 노드는 1과 4다.
진입차수가 0인 1과 4를 큐에 넣는다. 큐에 넣은 후 각각이 가리켰던 노드(3번 노드)의 진입차수를 하나씩 줄인다. 진입차수를 줄였더니 0이 되면, 그 진입차수가 0이 된 노드(3번 노드)를 큐에 넣는다. 또 다시 큐에 넣었던 노드를 꺼내면서 3번노드가 가리켰던 2번노드의 진입차수를 하나를 줄인다. 그리고 진입차수가 0이된 2번 노드를 다시 큐에 넣고 꺼내면서, 큐는 빈값이 되고 알고리즘은 끝이난다.

큐에 들어갔던 순서는 [1,4,3,2] 일 수도 있고, [4,1,3,2] 일 수도 있다. 위 예시에서는 1이 4보다 먼저와야하는지 4가 1보다 먼저와야하는지에 대한 순서는 정해주지 않았기 때문이다. 위상정렬은 이렇게 정렬된 답이 한개가 아닌 여러개가 나올 수 있다.

## 아이디어 떠올리기
위 문제를 읽을때, "A라는 문제는 B라는 문제보다 반드시 먼저 풀어야한다는 조건"을 보고 위상정렬 알고리즘을 떠올릴 수 있어야 한다. 또한 반드시 먼저 풀어야한다는 조건 + `가능하면 쉬운 문제를 먼저` 풀 것이라는 조건이 붙은 것을 보고 가장 쉬운문제는 숫자가 낮은 것이니까, `숫자가 가장 낮은것 부터 뽑는 최소 힙`을 사용해야겠다는 아이디어를 떠올려야 한다. 즉 문제의 이러한 조건을 보고 위상정렬과 최소 힙을 사용해서 풀어야겠다는 아이디어를 떠올려야 문제를 해결할 수 있다.

## 초기변수
위상정렬을 사용해야 하기 때문에, 노드가 가리키는 방향에 대한 정보를 담을 공간인 변수(graph)를 선언해줘야하고, 진입차수에 대한 정보를 담을 공간인 변수(indegree)를 선언해야 한다. 그리고 heap을 사용할 것이기 때문에 힙을 사용할 리스트를 선언한다.

```python
Indegree = [0*(n+1)] #진입차수 구해서 넣을 저장공간
Graph = [[] for _ in range(n+1)] # 노드가 가리키는 방향에 대한 정보를 담을 공간
Heap = []
```

## 풀이과정
진입 차수가 0인 것 부터 힙에 담고, 힙에서 최소값부터 하나씩 뽑으면서 진행한다.(문제에서 난이도가 낮은 문제 우선순위라 했으니, 숫자가 작은 수를 우선으로 뽑을 수 있는 최소 힙을 사용한다)

숫자를 뽑는 동시에, 그 숫자가 가리키는 간선을 제거한다. 이 말은 즉, 그 숫자가 가리키는 숫자의 진입차수를 하나줄인다. 진입차수가 하나 줄었다는 것은 자기를 가리켰던 노드 하나가 앞에서 사용되었다는 의미로도 볼 수 있겠다. 

이런식으로 진입차수가 전부 줄어 0이 되면 더 이상 자기보다 우선적으로 나와야하는 노드가 없어짐을 의미할 것이다. 따라서 진입차수가 0인 노드를 힙에 담아 다시 숫자가 작은것부터 뽑도 록 하여 반복해서 진행하면 된다.

## 풀이 간단하게 정리
1.일단 진입차수가 0인 노드들을 모두 최소힙에 담고 시작한다.

2.힙에 담은 노드들 중 숫자가 작은것을 먼저 뽑아서 정답 리스트(results)에 담고, 그 노드가 가리켰던 노드의 진입차수(indegree[i])를 하나 줄인다.

3.2번에서 줄였는데 진입차수가 0이 되었다면, 힙에담는다.

4.2번-3번 과정을 반복하다가 힙에 더 이상 노드가 없으면 반복을 끝내고 결과를 출력한다.

## 제출코드
```python
import heapq

N, M = map(int,input().split())

graph = [[]for _ in range(N+1)]
indegree = [0]*(N+1)
min_heap = []

for _ in range(M):
    a, b = map(int,input().split())
    graph[a].append(b)
    indegree[b] += 1

for i in range(1,N+1):
    if indegree[i] == 0:
        heapq.heappush(min_heap, i)

results = []

while min_heap:
    pop_ = heapq.heappop(min_heap)
    results.append(str(pop_))
    for item in graph[pop_]:
        indegree[item] -= 1
        if indegree[item] == 0:
            heapq.heappush(min_heap, item)
            
print(' '.join(results))
```