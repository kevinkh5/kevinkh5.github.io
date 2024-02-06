---
title: '[DFS] 프로그래머스 여행경로 풀어보기(python)'
author: baduk
date: 2024-02-06 15:22:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:
<https://school.programmers.co.kr/learn/courses/30/lessons/43164>


## 문제소개
항공권을 모두 써서 여행경로를 짜려하고, ICN에서 출발한다. 모든 공항경로를 배열에담아 출력해라.

## 문제접근 및 풀이
문제를 봤을 때, DFS탐색으로 크게 어렵지 않게 풀 수 있을 거라고 생각했는데, 생각보다 문제를 풀면서 헤맸다.

먼저 문제의 조건으로 공항에 알파벳 순서가 앞서는 순서로 경로를 리턴해야하므로, 정렬을 수행해야 했고, 나머지는 dfs 함수 스택방식으로 해결했다.  

ICN부터 스택에 넣고, 이 꼭대기에 있는 ICN을 가지고 딕셔너리에 ICN과 연결된 다른 공항을 알파벳 순으로 pop시켜서 스택에 append한다. 그리고 다시 꼭대기에 있는 공항으로 딕셔너리에 접근하여 공항을 또 pop시키고 스택에 넣는다. 이러한 과정을 반복하다가 더 이상 딕셔너리에 접근했을 때 pop할 공항없이 빈리스트면 stack 꼭대기 부터 result리스트에 append 한다.

그리고 최종적으로 result리스트에 있는 요소들을 거꾸로 출력하면 답이 된다.

## 제출코드
```python
from collections import defaultdict

def solution(tickets):
    adj = defaultdict(list)
    
    # 출발->도착 관계표현
    for ticket in tickets:
        adj[ticket[0]].append(ticket[1])
        
    # 정렬    
    for i in adj.keys():
        adj[i].sort(reverse=True)
    
    # DFS 스택방식
    results = []
    def dfs(start):
        stack = [start]
        while stack:
            start = stack[-1]
            if adj[start] == []:
                results.append(stack.pop())
            else:
                stack.append(adj[start].pop())
                
    dfs('ICN')
    return results[::-1]
```