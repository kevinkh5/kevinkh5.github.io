---
title: '[Breadth First Search] 너비우선탐색(BFS) 알고리즘을 파이썬으로 구현해보자'
author: baduk
date: 2023-12-31 20:10:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Breadth First Search, BFS, python, problem solving, algorithm]
use_math: true
---
## 너비우선탐색(BFS:Breadth Frist Search)이란?
너비우선탐색 알고리즘은 이름 그대로 너비를 우선적으로 탐색하는 알고리즘이다. 예를 들어 아래 사진과 같은 이진트리가 있으면 루트노드인 47부터 시작해서 47의 자식노드인 21, 76을 큐에 넣고 21을 꺼내면서 21의 자식노드인 18,27를 또 큐에 넣는다. 그리고 47의 오른쪽 자식이었던 76을 꺼내면서 76의 자식노드인 52와 82를 또 다시 큐에 넣는다. 그리고 큐에 들어간 나머지를 차례대로 꺼내오면서 너비우선탐색이 끝나게 될 것이다. (18,27, 52, 82는 자식노드가 없기 때문에 큐에는 더 이상 노드가 추가되어 들어가지 않을것이다.)

<img src = "https://lh3.googleusercontent.com/pw/ABLVV85b8P-xV1wjk_jNMu9QkYRqm41XsYGOOC21tbNshIZfHZFZAvNTKwHkxWx5buI3EHEhAbU9KxsFPSy_wWKP9189-mYLjN0h6LmP3ny44Nvwp1NfScP6U3E9qorFWTB4ibBIqkX2NxoFOYhHLf3YnGMp=w1554-h1167-s-no-gm?authuser=0" width="400" height="400">

따라서 위 이진트리의 BFS 탐색 순서는 `47 -> 21 -> 76 -> 18 -> 27 -> 52 -> 82` 가 될 것이다.

## 너비우선탐색을 파이썬으로 구현해보자
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
    
class BinarySearchTree:
    def __init__(self):
        self.root = None
        
    def insert(self, value):
        new_node = Node(value)
        if self.root == None:
            self.root = new_node
            return True
        temp = self.root
        while True:
            if new_node.value == temp.value:
                return False
            elif temp.value > new_node.value:
                if temp.left == None:
                    temp.left = new_node
                    return True
                temp = temp.left
            elif temp.value < new_node.value:
                if temp.right == None:
                    temp.right = new_node
                    return True
                temp = temp.right
```
먼저 너비우선탐색을 할 때 이진트리에서 진행할 것이므로 이진트리의 constructor와 insert메소드를 위와 같이 작성하였다.

너비우선탐색 알고리즘을 코드로 구현해보면 아래와 같다.
```python
    def BFS(self):
        current_node = self.root
        queue = []
        results = []
        queue.append(current_node)
        
        while len(queue) > 0:
            current_node = queue.pop(0)
            results.append(current_node.value)
            if current_node.left is not None:
                queue.append(current_node.left)
            if current_node.right is not None:
                queue.append(current_node.right)
        return results
```
루트노드를 시작으로 하면서 루트노드의 왼쪽 자식노드와 오른쪽 자식노드를 queue에 넣어준다. 그리고 꺼내온 루트 노드는 result리스트에 넣어주고 다시 queue에 넣어줬던 노드를 꺼내와서 그 노드의 자식노드를 queue에 넣고 꺼낸 노드는 result리스트에 넣어준다. 이 과정을 queue에 더 이상 노드가 없을 때까지 반복한다. 그러면 결과적으로 얻은 result 리스트에는 탐색한 노드가 0번 인덱스부터 순차적으로 들어가 있는 것을 확인할 수 있다.

이렇게 너비우선탐색 알고리즘을 파이썬으로 구현해보았다.