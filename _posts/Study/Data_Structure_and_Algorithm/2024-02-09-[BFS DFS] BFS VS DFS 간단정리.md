---
title: '[BFS DFS] BFS VS DFS 간단정리'
author: baduk
date: 2024-02-09 19:17:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
알고리즘 문제중 그래프 탐색 문제를 풀다보면 BFS, DFS 둘다 풀리는 경우도 있지만, 둘 중 하나를 잘 골라서 써야 효율성 통과가 되는 경우가 있다. 아래는 상황별로 BFS를 쓸지 DFS를 쓸지 잘 골라서 쓰는 예시다.

## 상황별 BFS, DFS 골라쓰기
최단경로 찾아야 하는 경우 -> BFS 사용하기

레벨 별 탐색 해야하는 경우 -> BFS 사용하기

노드의 깊이가 낮고 검색대상의 규모가 작은 경우 -> BFS 사용하기

노드의 깊이가 깊은 것을 빨리 찾아야 하는 경우 -> DFS 사용하기

모든 노드를 다 탐색해야하는 경우 -> BFS/DFS 아무거나 사용하기 (난 구현이 익숙한 BFS선호)


