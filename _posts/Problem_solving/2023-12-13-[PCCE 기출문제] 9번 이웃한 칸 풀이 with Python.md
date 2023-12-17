---
title: '[PCCE 기출문제] 9번 이웃한 칸 풀이 with Python'
author: baduk
date: 2023-12-13 17:26:00 +0900
categories: [Problem solving, PCCE]
tags: [PCCE, Programmers, Problem Solving]
---
문제링크:
<https://school.programmers.co.kr/learn/courses/30/lessons/250125/>
## 소감
위 문제는 프로그래머스 LV1 문제로 남들은 쉽다할 수 있지만 나한테는 그렇게 쉽다고 느껴지지 않았다. 문제풀이 많이 해야겠다.

## 풀이
나는 어떠한 알고리즘을 사용하지 않고 단순 구현으로 해결했다. 특별히 시간복잡도를 준수해야하는 문제는 아니었던것 같다. 

![photo](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/cb8c0433-a307-4184-b224-6185c87dfc07/9-1.jpg)

위 사진에서 특정한 칸에 대해서 좌우상하로 같은 것이 있는지 검사만 하도록 로직을 작성하였다.

좌우상하 같은지 검사하는 건 늘 그렇듯 for 반복문으로, h에서 -1해주거나 +1해주거나 또는 w에서 -1해주거나 +1해주거나. 각각 한 칸에 대해서 4번 검사를 해주면 된다.


그걸 코드상에서는 
```python
v = [(0,-1),(0,1),(-1,0),(1,0)]
count = 0
for _ in v:
    new_h = h + _[0]
    new_w = w + _[1]
    if valid(board,new_h,new_w):
        if board[h][w] == board[new_h][new_w]:
            count += 1
```
위와 같이 이렇게 구현하였다.

for문 안쪽에 valid라는 함수를 사용하고 있는데 valid함수는 이름 그대로 유효한 위치인지 아닌지를 판단해주는 함수이다. 예를 들어 h가 0이면 h-1을 해주었을 때, -1이 되어서 유효하지않은 h값이 되게되고 따라서 유효하지 않은 h값으로 리스트에 접근하면 out of index 에러가 발생할 것이다. 따라서 valid함수로 사전에 이러한 에러를 방지하도록 하였다.

## 전체코드
```python
def solution(board, h, w):
    def vaild(board,h,w):
        if (h > -1 and h < len(board)) and (w > -1 and w < len(board[0])):
            return True
        else:
            return False
        
    v = [(0,-1),(0,1),(-1,0),(1,0)]
    
    count = 0
    for _ in v:
        new_h = h + _[0]
        new_w = w + _[1]
        if vaild(board,new_h,new_w):
            if board[h][w] == board[new_h][new_w]:
                count += 1
    return count
```