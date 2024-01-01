---
title: '[Depth First Search] 깊이우선탐색(DFS) 알고리즘을 파이썬으로 구현해보자'
author: baduk
date: 2023-12-31 21:37:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Depth First Search, DFS, python, problem solving, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>
     
## 깊이우선탐색(DFS)알고리즘 이란?
깊이우선탐색 알고리즘은 이름 그대로 깊숙하게 먼저 탐색하는 알고리즘이다.

깊이우선탐색 알고리즘 중에서도 `PreOrder`인 경우는 루트노드부터 시작하여 Root-Left-Right로 순회를 한다. 예를들어 아래와 같은 이진트리가 있다고 했을 때, 루트노드인 47부터 시작하고, 21을 지나 18을찍고 다시 21로 돌아온 후 21의 오른쪽 노드인 27을 순회한다. 그리고 다시 47로 돌아와서 순회하지않았던 47의 오른쪽노드인 76노드를 찍고 76의 왼쪽 노드와 오른쪽 노드를 모두 거쳐서 다시 루트노드로 돌아온다.

<img src = "https://lh3.googleusercontent.com/pw/ABLVV84BHC74ooEENbr80q2R2QxdGsskwydXekBH7RHmYHMKJh7uIJLH0oqUycWTgKMlw6xIEFCe3MTEizDaPLtgCrDEyrll7eLTI1VX4vYP9ERLKzJQVUv_I_937vbziElGeRXeAgqw53gTkSvvbahfqr5g=w1271-h1137-s-no-gm?authuser=0" alt="exam" width="400" height="400">

따라서 `47 -> 21 -> 18 -> 27 -> 76 -> 52 -> 82` 순으로 노드를 순회한다.

## DFS(PreOrder)를 코드로 구현해보자

```python
    def dfs_pre_order(self):
        results = []
        
        def traverse(current_node):
            results.append(current_node.value)
            if current_node.left is not None:
                traverse(current_node.left)
            if current_node.right is not None:
                traverse(current_node.right)
                
        traverse(self.root)
        return results
```
먼저 노드가 순차적으로 들어갈 공간인 results 리스트를 선언한다. 그리고 아래 traverse함수에서는 재귀적으로 함수가 불려지면서 DFS가 진행된다. 

먼저 cuurent_node는 루트노드가 될 것이고, 왼쪽 자식노드가 있으면 traverse가  왼쪽 자식노드가 없을 때 까지 재귀적으로 불려진다. 그렇게 가장 깊이 있는 왼쪽 자식까지 도달하고 다시 함수를 빠져나오면서 오른쪽 자식노드가 있는지 확인하고 있으면 오른쪽 자식노드가 없을 때 까지 다시 재귀적으로 함수가 불려진다. 그렇게 왼쪽 오른쪽 자식노드가 다 불려지고 나서는 아직 확인되지않은 오른쪽 자식노드 까지 쭉 같은 방식으로 순회하면서 깊이우선탐색이 마무리된다.

## DFS(PostOrder)를 코드로 구현해보자
위에서는 루트노드부터 result리스트에 삽입하면서 순회를 시작하는 PreOrder에 대해서 다뤄보았다. 이와달리 PostOrder는 루트노드를 순회의 맨마지막으로 한다. 즉, PostOder는 순서가 Left-Right-Root가 된다. 코드는 아래와 같다.
```python
    def dfs_post_order(self):
        results = []
        
        def traverse(current_node):
            if current_node.left is not None:
                traverse(current_node.left)
            if current_node.right is not None:
                traverse(current_node.right)
            results.append(current_node.value)
                
        traverse(self.root)
        return results
```
PreOrder와의 차이는 단지 results 리스트에 노드를 담는 타이밍이다. PostOrder는 왼쪽, 오른쪽 노드가 없는 시점부터 results 리스트에 노드를 담는다. 따라서 PostOrder의 경우 위 그림의 이진트리를 탐색 했을 때 노드의 순서가 이와 같이 될 것이다. ->[18, 27, 21, 52, 82, 76, 47]

## DFS(InOrder)를 코드로 구현해보자
InOrder의 경우, PreOrder,PostOrder와 달리 루트노드의 순서가 중간에 온다. 순회 순서는 Left-Root-Right가 된다.
```python
    def dfs_in_order(self):
        results = []
        
        def traverse(current_node):
            if current_node.left is not None:
                traverse(current_node.left)
            results.append(current_node.value)
            if current_node.right is not None:
                traverse(current_node.right)
            
        traverse(self.root)
        return results
```
InOrder의 코드도 PreOrder, PostOrder와 거의 똑같다. 차이가 있다면 left노드의 탐색이 끝나고나서 results리스트에 노드를 삽입한다는 점이다. InOrder의 경우 위 그림의 트리에서 탐색했을때 노드의 순서는 이와 같이 될 것이다.-> [18, 21, 27, 47, 52, 76, 82] 



이렇게 해서 너비우선탐색(DFS)알고리즘을 PreOrder(Root-Left-Right), PostOrder(Left-Right-Root), InOrder(Left-Root-Right)로 나누어 각각 파이썬으로 구현해보았다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>