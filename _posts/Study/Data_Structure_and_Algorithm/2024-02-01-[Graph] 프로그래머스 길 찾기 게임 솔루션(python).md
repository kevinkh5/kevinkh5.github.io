---
title: '[Graph] 프로그래머스 길 찾기 게임 솔루션(python)'
author: baduk
date: 2024-02-01 12:01:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:<https://school.programmers.co.kr/learn/courses/30/lessons/42892>

## 문제소개
좌표로 표현한 트리를 데이터로 표현하여 전위 순회, 후위 순회에 대한 결과를 출력하는 문제

## 문제접근 및 풀이
이 문제의 핵심은 좌표를 가지고 트리를 어떻게 구성할 것인지 생각하는 부분이다. 먼저 파악해야할 것은 y좌표는 트리의 높이인 레벨을 뜻하고, x좌표는 노드가 가진 크기라고 생각할 수 있어야 한다. 그리고 한 가지 더 알아야 할 것은 노드의 이름이 nodeinfo라는 리스트안에서 놓여진 순서로 결정된다는 사실이다.

따라서 이 트리를 구성할 때, 각 노드가 (x, y, 노드이름) 이 3가지 정보를 포함하면서 트리를 구성해야 한다. 즉, 이 부분은 class 구현이 필요하다.

한 노드당 가지는 정보가 x,y,노드이름,왼쪽 자식 노드, 오른쪽 자식 노드 이므로 아래와 같이 클래스를 만들어준다.

```python
    class Node:
        def __init__(self, node, x, y):
            self.node = node
            self.x = x
            self.y = y
            self.left = None
            self.right = None
```

그리고 각 노드들을 이진탐색트리에 삽입해야 하므로 이진탐색트리에 대한 클래스 구현과 insert메소드 구현이 필요하다. 따라서 아래와 같이 코드를 작성한다.

```python
   class Bst:
        def __init__(self):
            self.root = None

        def insert(self, node, x, y):
            new_node = Node(node, x, y)
            if self.root == None:
                self.root = new_node
                return True
            temp = self.root
            while True:
                if new_node.x == temp.x:
                    return False
                elif new_node.x < temp.x:
                    if temp.left == None:
                        temp.left = new_node
                        return True
                    temp = temp.left
                elif new_node.x > temp.x:
                    if temp.right == None:
                        temp.right = new_node
                        return True
                    temp = temp.right
```
위와 같이 클래스를 구현하면 먼저 Bst클래스에 대한 객체를 생성하고, 그 객체에 insert메소드에 접근하여 노드를 삽입할 수 있다. self.root가 None이면 즉, 아직 삽입된 노드가 없으면 삽입할 노드 자체가 root가 되고, 그 다음 부터는 각 노드가 가진 값의 크기를 비교해서 왼쪽 자식으로 붙일지 오른쪽 자식으로 붙일지를 결정하고 삽입한다.

이와 같이 트리를 구성하였다면, 이 트리를 순회하기 위해 깊이우선탐색 알고리즘을 이용하면 된다.

## 전체 코드
```python
import sys
sys.setrecursionlimit(10**6)

def solution(nodeinfo):
    class Node:
        def __init__(self, node, x, y):
            self.node = node
            self.x = x
            self.y = y
            self.left = None
            self.right = None

    class Bst:
        def __init__(self):
            self.root = None

        def insert(self, node, x, y):
            new_node = Node(node, x, y)
            if self.root == None:
                self.root = new_node
                return True
            temp = self.root
            while True:
                if new_node.x == temp.x:
                    return False
                elif new_node.x < temp.x:
                    if temp.left == None:
                        temp.left = new_node
                        return True
                    temp = temp.left
                elif new_node.x > temp.x:
                    if temp.right == None:
                        temp.right = new_node
                        return True
                    temp = temp.right

    def dfs(current_node):
        if current_node == None:
            return
        preorder_result.append(current_node.node)
        if current_node.left != None:
            dfs(current_node.left)
        if current_node.right != None:
            dfs(current_node.right)
        postorder_result.append(current_node.node)

    i = 0
    for node in nodeinfo:
        i += 1
        node.append(i)
    nodeinfo = sorted(nodeinfo,key=lambda x:(-x[1],x[0]))
    bst = Bst()

    for node in nodeinfo:
        x = node[0]
        y = node[1]
        n = node[2]
        bst.insert(n,x,y)

    preorder_result = []
    postorder_result = []
    dfs(bst.root)

    return [preorder_result,postorder_result]
```

## 후기
이 문제는 사실 이진탐색트리와 DFS를 잘 이해하고 있고, 구현까지 완벽하게 할 수 있으면 어려움없이 풀 수 있는 문제다. 하지만 class구현에 익숙하지 않다면 시간도 많이 걸리고, 어렵게 느껴질 수 있다.


