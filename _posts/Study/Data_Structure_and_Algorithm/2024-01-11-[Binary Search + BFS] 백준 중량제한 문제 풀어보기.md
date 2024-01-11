---
title: '[Binary Search + BFS] 백준 중량제한 문제 풀어보기'
author: baduk
date: 2024-01-11 15:39:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, algorithm, 이분탐색, 이진탐색, 너비우선탐색]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>
## 문제소개
섬이 N개 있고, 섬을 연결하는 다리가 M개 있다. 다리는 최대 C만큼의 무게를 견딜 수 있다. 특정 섬에서 다른 섬으로 물건을 한번에 옮기려는데, 옮길 수 있는 최대 무게구하면 된다.

## 아이디어 떠올리기

### 초기셋팅 with 인접행렬(adj)
일단 초기셋팅을 잘 해줘야 한다. 다리와 다리사이의 관계에 대한 정보를 담을 테이블이 필요하다.

예를 들어서 [1, 2, 3]과 같은 리스트는 1번섬과 2번섬 사이에 다리가 연결되어 있고, 다리의 중량제한이 3이라는 표현이라고 해보자.

[1, 2, 3]은 [2, 1, 3]과 같은 의미일 것이다. 따라서 이러한 입력을 받았을 때, 관계에 접근을 편하게 해줄 무언가 테이블이 필요할 것이다. 그 테이블은 파이썬의 리스트를 이용하면 된다.

만약 [1, 2, 3] 또는 [2, 1, 3]을 입력받아 아래 리스트 처럼 만들 수 있다면 관계에 대한 접근이 훨씬 용이해질 것이다. 예를들면 1번 섬과의 관계에 있는 섬을 아래 리스트 주소 1로 접근하면 쉽게 얻어낼 수 있다. 이런식으로 이용할 수  있다.

`[ [], [(2,3)], [(1,3)], [], [], …]`

위 리스트를 보면 1번주소로 접근했을 때, 1번섬과 연결된 2번섬의2(첫번째 인덱스)와 중량제한 3(두번째 인덱스)을 가져올 수 있고, 마찬가지로 2번주소로 접근했을 때 2번섬과 연결된 1번섬의 1과 중량제한 3을 가져올 수 있다.

기본적으로 이러한 섬과 섬사이의 관계에 대한 데이터를 가져올 수 있는 리스트가 마련되어야 BFS를 사용하기전의 준비를 갖출 수 있다.

### 이분탐색 떠올리기
문제조건으로 다리(M)개수는 최대 10만이하이고, 중량제한(C)의 최대 값은 10억 이하다. 따라서 모든 중량제한에 대해 무작정 구하려고하면 10억*10만으로 수가 너무 크다. 이렇게 $O(M * C)$ 의 시간복잡도로 이 문제를 해결할 수가 없다.

이진탐색을 사용하면 $O(M * logC) $로 해결할 수 있는데, 이렇게 할 경우 30 * 10만 = 300만으로 해결 가능하다.(10억은 2의 30승 정도 되니까, log 10억은 30정도임.)

### BFS 떠올리기
이 문제를 풀때 우리는 어떠한 중량제한에 대해서 특정 섬에서 다른 섬으로 물건을 옮기는 것이 가능한지, 즉, 시작지점에서 도착지점까지 갈 수 있는지 없는지를 파악해야 한다. 시작지점이 만약 1번 섬이라고 하면 1번 섬과 연결된 모든 섬을 가져오고, 그 가져온 섬에서 또 다시 그 가져온 섬과 연결된 모든 섬을 가져오고 하는 식으로 탐색과정을 거쳐야 한다. 이때 우리는 BFS를 사용해야한다.

## 해결과정

1.이분탐색할 준비를 위해, 최소 중량제한과 최대 중량제한을 구한다.

2.최소 중량과 최대 중량의 중간지점을 mid로 잡고 이분탐색을 실시한다.

3.출발점으로부터 도착지점까지 탐색이 가능하면 중량제한을 조금 높여볼만 해지므로 최소 중량을 mid+1로 다시 업데이트 시켜서 진행해본다. 이때 mid값은 현재까지 구한 최대중량이므로 값을 따로 변수에 저장해놓는다.

4.출발점으로부터 도착지점까지 탐색이 불가능하다는 것은 중량제한의 문제가 있었던 것이므로 최대 중량을 mid-1로 낮춰서 탐색을 진행 해본다.

5.3번, 4번과정을 반복하다가 중량제한 최소값이 최대값보다 커지는 시점이 오면 중단되도록하고, 저장해놓은 최대중량을 출력하면 끝이난다.

## 후기
머리로는 해결하는 과정이 그려지긴 했으나, 일단 초기 세팅부분부터 코드를 어떻게 작성해야할지에 대해서는 감이 안왔다. 특히 인접행렬을 리스트로 만들어서 섬과 섬사이의 관계 데이터를 저장할 아이디어는 정말 떠올릴 수 없었다. 이러한 문제처럼 어떤 물체와 물체 사이의 관계표현이 필요한 문제에 대해서 인접리스트를 떠올려볼 수 있도록 해야겠다.

BFS 개념은 대략 머리로는 알고 있었으나, BFS문제 해결을 위한 코드 작성 경험이 부족해서 코드 작성할 때도 시간이 많이 걸렸다. 문제 많이 풀어봐야겠다.

## 제출코드
```python
from collections import deque

N,M = map(int,input().split())
adj = [[]]+[[] for i in range(N)]
min_ = float('inf')
max_ = 0

for _ in range(M):
    A,B,C = map(int,input().split())
    adj[A].append((B,C))
    adj[B].append((A,C))
    min_ = min(min_,C)
    max_ = max(max_,C)

start, end = map(int,input().split())

max_weight = min_
while min_ <= max_:
    mid = (min_ + max_) // 2
    q = deque([start])
    check = [False for i in range(N+1)]
    check[start] = True
    while q:
        p = q.popleft()
        for link, weight in adj[p]:
            if weight >= mid and check[link]==False :
                q.append(link)
                check[link] = True
    result = check[end]
    if result == True:
        min_ = mid + 1
        max_weight = mid
    else:
        max_ = mid - 1
print(max_weight)

```


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>