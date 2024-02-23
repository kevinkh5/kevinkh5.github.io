---
title: '[Brute-Force] 백준 3085번 사탕게임 Python풀이'
author: baduk
date: 2024-02-23 16:27:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:
<https://www.acmicpc.net/problem/3085>

## 문제 간단소개
사탕의 위치를 바꿀 수 있는 모든 상황을 다 생각해야하는 문제. 사탕의 위치를 바꿨을 때, 연속된 사탕의 최대개수를 구하라.

## 문제 풀이 및 접근
오늘 풀어본 사탕 게임 문제의 난이도는 실버2다. 체감상 전혀 실버2수준의 문제는 아닌것 같다. 복잡한 아이디어나 알고리즘을 요구하는 문제는 아니지만, 정말 탐색할 수 있는 모든 상황을 다 이해해야 풀 수 있는 문제다. 또한 그 상황에 대한 처리와 세부적인 조건을 코드로 구현해야하기 때문에 상당히 까다로웠다.

일단 핵심 아이디어는 정말 바꿀 수 있는 상황을 다 떠올려 보는 것이다. 그리고 사탕을 바꾸고나서 연속되는 사탕의 개수를 체크할 때, 체크할 행과 열이 어딘지를 명확히 알아야 한다.

사탕의 위치를 바꾸는 방법은 1행1열의 사탕부터 N행 N열까지 모든 위치의 사탕을 상하좌우로 한번씩 다 바꿔가면서 체크하는 방법이 있다.

물론 이렇게 해도 문제는 풀리겠지만, for문을 사용하여 좌상부터 우하로 내려가기 때문에 굳이 상하좌우 네개를 확인할 필요는 없고, 사탕을 우하로 바꿔가면서 체크하면 된다.

## 사탕의 위치를 바꿨을 때 체크할 부분

사탕을 우(오른쪽)로 바꿨을 때(좌우교환)는 바뀐 위치, 즉 변동이 생긴 위치의 사탕에 대한 행과 열을 모두 검사하면 된다. 그럼 행 1개와 열 2개를 검사해야할 것이다.

사탕을 하(아래)로 바꿨을 때(상하교환)는 행 2개와 열 1개를 검사해야 할 것이다.

<img src='https://lh3.googleusercontent.com/pw/ABLVV85-8b6jSzBkkgkUbVANmmB3BV4ci-JT-OLysjP3ZDF_k5GQahwNBVruUSNjzo4jwfl7CDodXLs2f-7_1ISrs6tdPbRNTV6lmIiD2mffbjuzqfoKtf2QEGlfnpnSg3oXw2-wQe6SwGH5CN_sdD58j-Hq=w1008-h768-s-no-gm?authuser=0' width=500 alt='exam'>

이런식으로 모든 사탕을 바꿔가면서 체크하면 된다.

이때 필요한 것은 연속된 사탕의 개수를 구하는 함수가 필요할 것이고, 또 한가지 주의해야 할 점은 범위를 벗어났을 때의 상황에 대한 처리도 잘 해주어야 한다.


## 제출코드

```python
import sys
input = sys.stdin.readline

n = int(input())
arr = [list(map(str,input().rstrip())) for i in range(n)]+[['']*n]

def row_check(r):
    max_count = 0
    current_count = 0
    for i in range(n):
        if i == 0 or arr[r][i] == arr[r][i-1]:
            current_count += 1
        else:
            max_count = max(max_count, current_count)
            current_count = 1
    return max(max_count,current_count)

def col_check(c):
    max_count = 0
    current_count = 0
    for i in range(n):
        if i == 0 or arr[i][c] == arr[i-1][c]:
            current_count += 1
        else:
            max_count = max(max_count,current_count)
            current_count = 1
    return max(max_count,current_count)

max_count=1
for i in range(n):
    for j in range(n):
        if j < n-1:
            arr[i][j],arr[i][j+1] = arr[i][j+1],arr[i][j]
            row1=row_check(i)
            col1=col_check(j)
            col2=col_check(j+1)
            arr[i][j], arr[i][j + 1] = arr[i][j + 1], arr[i][j]
        else:
            row1=1
            col1=1
            col2=1
            
        if i < n-1:
            arr[i][j], arr[i+1][j] = arr[i+1][j], arr[i][j]
            row2=row_check(i)
            row3=row_check(i+1)
            col3=col_check(j)
            arr[i][j], arr[i+1][j] = arr[i+1][j], arr[i][j]
        else:
            row2=1
            row3=1
            col3=1
        max_count = max(max_count,row1,row2,row3,col1,col2,col3)

print(max_count)
```