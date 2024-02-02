---
title: '[Recursion] 프로그래머스 하노이의 탑 문제 풀어보기'
author: baduk
date: 2024-02-02 12:30:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
## 문제 소개
n개의 원판을 가진 하노이의 탑을 1위치에서 3위치로 옮길때 프로세스를 리스트에 담아 리턴해라

## 문제 접근 및 풀이
이 문제를 재귀 방법을 사용하여 해결해야 한다는 것을 알면서도 어떻게 코드를 작성해나가야할지 감이 잡히지 않았다.

먼저 중요한것은 하노이 탑의 원판 개수를 받는 변수(n), 어느 위치에서(start) 어느 위치로(goal) 옮길지에 대한 변수, 목적위치에 옮기기 위해 잠시 원판을 둘 위치에 대한 변수(sub)를 함수안에서 어떻게 적용시킬지를 고민해야 한다.

핵심 아이디어는 다음과 같다. 먼저 원판이 한개만 있다고 생각해보자. 그럼 원판을 1위치에서 3위치로만 옮기면 된다. 따라서 원판이 한개일 경우 굳이 목적위치에 옮기기 위해 잠시 원판을 둘 곳이 필요없이 시점위치와 목적위치만 있으면 될 것이다.

```python
    def hanoi(n,start,goal)
        if n == 1:
            result.append([start, goal])
            return
```
따라서 위 코드 처럼 단순히 result 리스트에 [start, goal] 이런식으로 삽입하면 될 것이다.

하지만 우리가 수행해야할 하노이 탑의 원판은 1개 뿐만 아니라 2개이상이 될 수 있다.

따라서 이번에는 좀 더 확장해서 원판이 두개 라고 생각해보자. 그럼 모든 원판을 1위치에서 3위치로 옮기기 위해서는 먼저 1위치에 있는 원판 하나를 뽑아 2위치에 옮겨야 한다. 이때 2위치는 목적위치가 아닌 서브위치라고 이름 짓자. 이처럼 원판이 한 개일 때와는 달리 원판이 두개 이상이 되면 서브위치라는 변수가 또 필요하다.

2개의 원판을 1->3으로 옮기는 과정안에는 1개의 원판을 1->2로 옮기는 과정이 필요하다. 따라서 아래와 같이 원판의 개수(n)를 1개 줄이고 처음에 받았던 sub변수를 함수의 goal파라미터에 넣어 재귀호출을 진행한다.
```python
    def hanoi(n,start,goal,sub):
        if n == 1:
            result.append([start, goal])
            return
        hanoi(n-1,start,sub,goal)
```
그럼 result 리스트에 1->2로 옮기는 과정에 대한 리스트가 삽입될 것이다.

그리고 재귀를 탈출하고 나서는 1에 있던 원판을 3으로 옮겨야 하므로 아래와 같이 코드를 추가한다.
```python
    def hanoi(n,start,goal,sub):
        if n == 1:
            result.append([start, goal])
            return
        hanoi(n-1,start,sub,goal)
        result.append([start, goal]) # 추가
```

그리고 다시 2위치에 임시로(sub) 두었던 원판을 목적지인 goal위치로 이동시키도록 작성한다. 코드는 아래와 같다.

```python
    def hanoi(n,start,goal,sub):
        if n == 1:
            result.append([start, goal])
            return
        hanoi(n-1,start,sub,goal)
        result.append([start, goal])
        hanoi(n-1,sub,goal,start) # 추가
```
이와같이 하노이 탑 프로세스를 재귀함수로 구현해봤다. 코드는 사실 짧고 간단하지만, 재귀를 적절히 사용하는 방법에 익숙하지 않다면 작성하기 어려울 수 있는 것 같다.

## 최종 제출 코드
```python
def solution(n):
    result = []
    def hanoi(n,start,goal,sub):
        if n == 1:
            result.append([start, goal])
            return
        hanoi(n-1,start,sub,goal)
        result.append([start, goal])
        hanoi(n-1,sub,goal,start)
    hanoi(n,1,3,2)    
    return result
```