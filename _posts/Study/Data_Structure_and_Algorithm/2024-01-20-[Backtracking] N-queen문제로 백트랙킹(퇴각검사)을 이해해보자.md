---
title: '[Backtracking] N-queen문제로 백트랙킹(퇴각검사)을 이해해보자'
author: baduk
date: 2024-01-20 11:49:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
## 백트랙킹(Backtracking)이란?
백트랙킹은 문제 해결을 위한 한 전략 기법중 하나다. 백트랙킹 기법을 간단하게 설명하자면 특정 조건에 부합하는 결과를 DFS(깊이우선탐색)방식으로 찾아나가다가 한 조건이 맞지 않으면, 뒤로 퇴각한 후에 다시 탐색하는 기법이다. 퇴각한 후 새로운 루트로 조건에 만족하는 루트를 찾아나가다가 결과적으로 모든 조건에 부합하였다면 해당 결과물을 얻어낸다.

여기서 루트가 조건에 맞는지 검사하는 것을 Promising이라고 하고, 조건에 맞지 않아 퇴각하는 것을 Pruning(가지치기)라고 한다. 백트랙킹 기법을 더 잘 이해하기 위해서는 N-queen문제를 푸는 과정에서 백트랙킹이 어떻게 적용되는지 살펴보면 될 것이다.

## N-queen문제 간단소개
N-queen 문제는 N x N 체스판이 있을 때, N개의 퀸이 서로를 공격할 수 없는 상태로 놓을 수 있는 경우를 모두 찾는 문제다.

여기서 조건은 "서로를 공격할 수 없는 상태" 즉, 서로가 대각선관계에도 놓일 수 없고, 수직, 수평관계에도 놓일 수 없는 관계에 놓여야 한다는 것이다.

아래 사진은 N=4일때 조건을 만족한 케이스다. 즉 4 x 4 체스판에 4개의 퀸이 서로가 공격할 수 없는 상태로 놓여있는 상황을 만족한 경우다.

<img src="https://lh3.googleusercontent.com/pw/ABLVV84aiKlLatq1c8FbjBsmfWas0JrAIpUCmSvIxz2GuKVhxyAxPsNxqhZyHR5hukZ9B0guFEXe-x_njmwyoP4e9vjDGpfz3sQW4T5FTyRo2k9KKMICfFwty8kZGGmaZEcFpPxapblYGU31O7BW9uPcyJcA=w1194-h1153-s-no-gm?authuser=0" width=300 alt='exam'>

## 문제 접근 아이디어
일단 체스판을 코드로 어떤식으로 입력하고 어떤식으로 결과를 받을지에 대해서 고민해 볼 필요가 있을 것이다. 나는 위 그림과 같은 결과를 아래와 같이 표현할 것이다.

```
[1, 3, 0, 2]

| |O| | |
| | | |O|
|O| | | |
| | |O| |
```
퀸의 위치를 행렬(1차원 리스트)로 표현하였고, 따라서 퀸의 위치는 0행1열, 1행3열, 2행0열, 3행2열에 있는 것이다.

그리고 탐색하는 과정은 0행0열부터 시작하고, 해당 row에 퀸을 놓을 수 있는 조건에 부합하면 list에 해당 column을 append하고, 다음 row로 이동하여 첫번째 column부터 마지막 column까지 탐색을 시도하면서 조건에 부합하면 list에 append하고 또 다음 row로 이동하면서 계속 탐색하도록 하면된다.

위와 같은 탐색은 DFS방식으로 진행할 것이며, 따라서 재귀 호출방식을  통해 탐색해야 할 것이다.

만약 탐색하는 과정에서 퀸을 놓을 다음 row에 대한 모든 column에 퀸을 놓을 수 없다면, 즉 모든 column에 대해서 조건에 만족하지 않으면 list에 있던 마지막 요소(임시적으로 퀸을 놓은 칼럼)를 pop시켜서 빼주고, 그 이전 행부터 다시 조건에 맞는 칼럼을 탐색해야 한다. 이 부분이 백트랙킹 기법이 적용된 부분이라고 할 수 있다.

위와 같은 모든 경우에 대한 과정을 거치면, 결과적으로 조건을 모두 만족한 칼럼이 리스트에 쌓여져있고, 그것을 또 다른 리스트에 저장해 놓으면, N x N체스판에 N개의 퀸을 놓을 수 있는 모든 경우에 대한 결과를 얻을 수 있다.

## 솔루션 코드
위 내용을 바탕으로 파이썬 코드로 구현해보자면 아래와 같다.
```python
def is_available(candidate, current_col):
    current_row = len(candidate)
    for queen_row in range(current_row):
        if candidate[queen_row] == current_col\
                or abs(candidate[queen_row] - current_col) == current_row - queen_row:
            return False
    return True


def dfs(n, current_row, current_candidate, final_result):
    if current_row == n:
        final_result.append(current_candidate[:])
        return

    for candidate_col in range(n):
        if is_available(current_candidate, candidate_col):
            current_candidate.append(candidate_col)
            dfs(n, current_row + 1, current_candidate, final_result)
            current_candidate.pop()


def solve_n_queens(n):
    final_result = []
    dfs(n, 0, [], final_result)
    return final_result

print(solve_n_queens(4))
# 출력:
# [[1, 3, 0, 2], [2, 0, 3, 1]]
```
먼저 solve_n_queens함수에 n을 입력하여 실행하면서, dfs탐색이 시작된다. 

dfs 함수 입력으로는 체스판의 사이즈와 개수를 나타내는 n과 탐색을 시작할 row(처음에는 0행부터 탐색을 시작해야하므로 0을 입력하여 실행), 퀸을 임시적으로 저장할 공간인 빈 리스트, 그리고 마지막으로 조건을 모두 만족했을 때 결과를 저장할 final_result가 입력으로 들어간다.

dfs함수 내부에서는 특정 행에 대한 0부터 ~ n-1까지에 대한 칼럼이 재귀호출 방식을 통해 탐색된다. 탐색과정에서 is_available함수(조건에 부합하면 True를 반환하고, 그렇지 못하면 False를 반환하는 함수)로 부터 True를 반환 받으면, 조건을 만족한 것이므로 정답이 될 가능성이 있는 퀸을 임시적으로 저장할 리스트인 current_candidate에 조건을 만족했던 칼럼을 append한다. 만약 칼럼이 만족하지 못했으면 if문을 건너뛰고 다음 칼럼으로 넘어가서 탐색한다.

is_available함수 내부에서는 체스가 수직(세로)관계에 놓여져 있는지, 대각선 관계에 있는지를 검사한다. 어차피 마지막으로 임시적으로 놓여진 퀸의 위치보다 다음 row를 기준 잡아 해당 column이 조건을 만족하는지를 체크할 것이므로, 수평(가로)검사는 하지 않아도 될 것이다.

수직관계에 놓이는 경우는 두 칼럼이 같은 경우이므로, 두 칼럼이 같은지 체크하고, 대각선 관계는 두 로우와의 차이(절대값)와 두 칼럼과의 차이(절대값)가 같으면 대각선 관계에 놓인 것 이므로, 두 로우의 차이에 대한 절대값과 두 칼럼과의 차이에 대한 절대값을 비교하여 available한지 체크한다.

모든 행에 대한 탐색이 끝나면 current_row가 n과 같아지는 순간이 오는데, 그때는 final_result리스트에 조건을 모두 만족한 결과물을 append시켜 저장한다.

