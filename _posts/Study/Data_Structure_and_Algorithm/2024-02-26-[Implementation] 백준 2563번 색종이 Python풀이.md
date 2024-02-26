---
title: '[Implementation] 백준 2563번 색종이 Python풀이'
author: baduk
date: 2024-02-26 15:49:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:
<https://www.acmicpc.net/problem/2563>

## 문제 간단소개
100X100 흰색 도화지위에 10X10 사이즈의 검은색 색종이를 N개 놓을 때, 검은색 영역의 넓이 구하기

## 문제 접근 및 풀이
보통 문제에 그래프가 주어지면 이를 행렬(리스트)로 표현하여 해결한다. 이 문제도 마찬가지로 도화지를 어떻게 행렬로 표현할 것 인지와 색종이의 위치에 대한 정보를 입력으로 받았을 때, 어떻게 처리해야할 것인지를 고민하면서 접근해야 할 것이다.

풀이는 간단하다. 흰색 도화지를 0으로 채운 2차원 행렬 101행 101열로 초기화하고, 색종이가 놓여진 영역을 1로 채운 후, 그 1을 전부 합하면 검은색 영역을 구할 수 있다.

이 문제의 핵심은 도화지를 행렬로 만들어, 색종이를 도화지위에 올렸을 때의 영역을 행렬에 어떻게 처리를 할 것인지를 떠올리는 것이다.

로직은 아래와 같다.

색종이의 가로 세로길이가 10이므로 입력받은 지점으로 부터 뒤에 10개의 위치를 차례로 방문하도록 한다. 이때 방문한 위치의 값을 1로 업데이트하고 모든 색종이에 대해서 1로 업데이트 되었다면, 마지막에 다시 그 행렬에 접근해서 모든 1을 합하여 출력하면 된다.

## 제출코드
```python
import sys
input = sys.stdin.readline

arr = [[0]*101 for i in range(101)]
n = int(input())
for i in range(n):
    a,b = map(int,input().split())
    for j in range(a,a+10):
        for k in range(b,b+10):
            arr[j][k] = 1
total = 0
for i in arr:
    total += sum(i)
print(total)
```