---
title: '[Backtracking] 백준 15686번 치킨 배달 Python풀이'
author: baduk
date: 2024-02-16 17:19:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:
<https://www.acmicpc.net/problem/15686>

## 문제 간단소개
좌표상에 가정 집이 있고, 치킨집이 있다. 치킨집 M개만 남길 때, 모든 가정집은 자기집 입장에서 최소거리로 놓여진 치킨집과의 거리를 구한다. 이 거리를 치킨거리라고 했을 때, 이때 모든 집의 치킨거리를 합 했을때, 그 합의 최소값을 구하라.

## 문제 접근
나는 처음에 최소거리를 구해야한다 해서, BFS 풀이로 접근했다. 생각해보니 치킨집 좌표와 가정집 좌표를 안다고 했을 때, 굳이 BFS로 최소거리를 구할 필요없이, 문제에서 준 식에 대입해서 구하면 된다.

처음 풀이에서 조합(이터툴 콤비네이션)과 BFS를 활용해서 풀었는데, 시간초과가 났다.

결국 솔루션을 참고하였고, 나는 이 문제를 두 가지 방식으로 해결하였다. 첫번째는 백트랙킹을 활용한 브루트포스, 두번째는 조합을 활용한 부르트포스로 풀었다.

결과적으로는 백트랙킹 풀이의 속도가 조금 더 빨랐다.

## 백트랙킹 풀이
문제에서 주어진 것처럼 남겨둘 치킨집의 최대개수 M은 최대 13이다. 따라서 치킨집을 남길지 말지에 대한 모든 경우의 수가 2의 13제곱으로 시간복잡도가 그렇게 크지 않다. 따라서 백트랙킹 풀이를 이용해도 된다는 판단을 내릴 수 있다.

DFS함수를 하나 만들어서, 치킨집을 폐쇄시킬지 남겨둘지를 재귀적으로 호출하도록 한다. 만약 치킨집을 남겨둔다면 치킨집 좌표가 들어있는 리스트에서 치킨집 좌표 하나 추가해서 입력으로 넣어주고, 만약 치킨집 모든 좌표를 다 확인했으면, 더 이상 확인할 수 있는 치킨집 좌표가 없으므로 리턴시킨다. 이때 남겨둔 치킨집 좌표가 m개라면 그 시점에 있을 때 가정집과 치킨집사이의 거리를 확인해야 하므로 거리구하는 함수인 cal 함수를 호출하여 거리를 구한다.

이런식으로 치킨집의 모든 케이스에 따라서 구한 거리 중 최소거리를 출력하면 답이된다.

추가적으로 m개 이상의 치킨집을 남겨두는 경우는 없으므로 해당 케이스에 대해서는 21번라인에서 리턴하도록 가지치기 코드 넣었다.
```python
import sys
input = sys.stdin.readline
n,m = map(int,input().split())
graph = [list(map(int,input().split())) for i in range(n)]
chicken = []
home = []
for i in range(n):
    for j in range(n):
        if graph[i][j] == 2:
            chicken.append((i,j))
        if graph[i][j] == 1:
            home.append((i,j))

def dfs(start=0, tlst=[]):
    global final_min_dist
    if start == len(chicken):
        if len(tlst) == m:
            final_min_dist = min(final_min_dist,cal(tlst))
        return
    if len(tlst) > m:
        return
    dfs(start+1, tlst+[chicken[start]])
    dfs(start+1, tlst)

def cal(tlst):
    total_min_dist = 0
    for i in home:
        min_dist = float('inf')
        for j in tlst:
            dist = abs(i[0]-j[0]) + abs(i[1]-j[1])
            min_dist = min(min_dist,dist)
        total_min_dist += min_dist
    return total_min_dist

final_min_dist = float('inf')
dfs()
print(final_min_dist)
```

## 조합을 이용한 풀이
조합을 이용한 풀이는 사실 더 간단하다. 복잡하게 DFS함수 재귀적으로 호출 할 것 없이, 치킨 집을 남겨두는 모든 케이스를 Combinations로 가져와서 각각에 대해 거리 구하고 합하여, 그 중 최소 거리합을 구하면 끝이다. 풀이는 간단하나, 코드 작성시 헷갈릴 수 있으니 주의해야한다. 어떻게 비교하고 합하는지를 그림으로 확실히 그려놓고 이해하고 코드작성에 들어가야 덜 헷갈릴 것이다.

```python
import sys
input = sys.stdin.readline
from itertools import combinations

n,m = map(int,input().split())
graph = [list(map(int,input().split())) for i in range(n)]
chicken = []
home = []
for i in range(n):
    for j in range(n):
        if graph[i][j] == 1:
            home.append((i,j))
        if graph[i][j] == 2:
            chicken.append((i,j))
            
final_min_dist = float('inf')
for i in combinations(chicken,m):
    total_min_dist = 0
    for j in home:
        min_dist = float('inf')
        for k in i:
            dist = abs(j[0] - k[0]) + abs(j[1] - k[1])
            min_dist = min(min_dist, dist)
        total_min_dist += min_dist
    final_min_dist = min(final_min_dist, total_min_dist)

print(final_min_dist)
```

## 후기
조합을 이용한 풀이보다 백트랙킹을 이용하는 것이 조금 더 빨랐다. 조합을 이용한 풀이는 단순 구현의 느낌이 있어서, 좀 더 알고리즘을 활용하는 듯한 느낌인 백트랙킹 풀이가 개인적으로 더 나은 것 같다.