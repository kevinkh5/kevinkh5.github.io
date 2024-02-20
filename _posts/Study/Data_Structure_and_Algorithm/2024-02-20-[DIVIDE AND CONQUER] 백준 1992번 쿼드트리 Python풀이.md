---
title: '[DIVIDE AND CONQUER] 백준 1992번 쿼드트리 Python풀이'
author: baduk
date: 2024-02-20 22:21:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:
<https://www.acmicpc.net/problem/1992>

## 문제 간단소개
0과 1을 요소로 가지고 있는 행렬에서, 왼쪽 위,오른쪽 위,왼쪽 아래,오른쪽 아래 4개의 영역이 0 또는 1로 동일하다면 하나의 수로 압축시킬 수 있다. 압축시켰을 때의 결과를 출력하라.

## 문제 접근
이 문제를 풀기 위해서는 분할정복 알고리즘을 사용해야한다. 분할정복의 과정을 이해하는 것은 사실 크게 어렵지는 않지만, 아무래도 재귀호출을 이용하면서 작게 쪼개고 합치는 과정을 코드로 담아야 하다 보니 익숙하지 않다면 어려울 수 있을 것 같다.

분할정복의 재귀는 DFS나 백트랙킹 구현할 때 재귀를 구현하는 것 보다 좀 더 복잡한 것 같다.


## 풀이 과정

알고리즘을 말로 풀어서 설명해보자면 아래와 같다.

1.처음부터 행렬을 네 부분으로 나눠서 검사하는게 아니라 모든 행렬이 0으로 이루어졌거나 또는 1로 이루어졌을 수도 있으므로,전체행열을 다 검사한다는 느낌으로 체크해간다.

2.첫행첫열부터, 첫행첫열을 기준으로 다른것이 있는지 하나씩 검사해 나가는데

3.검사하다가 기준이 되는 것과 다른 값이 나오면, 압축할 수 없으므로, 괄호열고 다시 함수를 재귀 호출한다.

4.재귀 호출 할 때는 사이즈를 반 나눈것을 인자로 넣는다. 그 이유는 전체행렬의 1/4에 해당하는 부분을 검사하기 위함이다.

5.이제는 분할시킨 전체행렬의 1/4에 해당하는 행렬에 대해서 요소가 같은지를 또 검사한다. 즉, 1/4에 해당하는 행렬의 영역에 대해서 앞에 과정을 다시 반복한다. 반복하다가 사이즈가 1이 되면 더 이상 쪼갤 수 없으므로 bit 1개 문자를 ans변수에 더해주고 리턴 될 것이다. (참고로 행렬을 탐색하는 순서를 크게 보자면 왼쪽위부분 -> 오른쪽 위부분 -> 왼쪽 아래부분 -> 오른쪽 아래부분 이다.)

위 설명이 이해가 가지 않는다면, 아래 코드를 디버깅모드로 단계별로 확인해보면 이해하는데 도움이 될 것이다.

## 제출코드
```python
import sys
sys.stdin = open("input.txt", "r")
input = sys.stdin.readline
#
n = int(input())
arr = [list(map(str,input().rstrip())) for i in range(n)]
ans = ''
def quad(row,col,size):
    global ans
    bit = arr[row][col]
    half = size // 2
    for i in range(row, row + size):
        for j in range(col, col + size):
            if bit != arr[i][j]:
                ans += '('
                quad(row,col,half) # 좌상
                quad(row,col+half,half) # 우상
                quad(row+half,col,half) # 좌하
                quad(row+half,col+half,half) # 우하
                ans += ')'
                return
    ans += bit
quad(0,0,n)
print(ans)
```