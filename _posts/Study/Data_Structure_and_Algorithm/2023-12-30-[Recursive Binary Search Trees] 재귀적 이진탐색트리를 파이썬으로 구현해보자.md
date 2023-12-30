---
title: '[Recursive Binary Search Trees] 재귀적 이진탐색트리를 파이썬으로 구현해보자'
author: baduk
date: 2023-12-30 10:03:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Binary Search Trees, python, problem solving, algorithm]
use_math: true
---

## 이진트리의 Constructor 구현하기
먼저 이진트리의 Constructor와 insert 메소드는 아래와 같다.

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

## Contains 구현하기
Contain메소드는 특정 노드가 트리에 포함이 되어있는지 아닌지를 체크할 수 있는 메소드다. 찾고자 하는 노드가 포함이 되어있다면 True를 반환할 것이고, 없다면 False를 반환할 것이다.
```python
    def __r_contains(self, current_node, value):
        if current_node == None:
            return False
        if value == current_node.value:
            return True
        if value < current_node.value:
            return self.__r_contains(current_node.left, value)
        if value > current_node.value:
            return self.__r_contains(current_node.right, value)

    def r_contains(self, value):
        return self.__r_contains(self.root, value)
```
코드는 위와 같이 두개의 함수로 구성될 수 있고, 먼저 시작점으로서 r_contains가 불러지고 그 다음 __r_contains메소드가 불러지는데, 재귀적 호출은 __r_contains메소드에서 이루어진다. 찾고자 하는 노드를 찾을때까지 혹은 current_node의 위치가 None으로 갈 때 까지(이때는 찾고자 하는 노드는 없는 경우) 계속해서 호출하다가 True 또는 False를 반환하게된다.

## insert 구현하기(재귀이용)
```python
    def __r_insert(self, current_node, value):
        if current_node == None:
            return Node(value)
        if current_node.value > value:
            current_node.left = self.__r_insert(current_node.left, value)
        if current_node.value < value:
            current_node.right = self.__r_insert(current_node.right, value)
        return current_node
        
    def r_insert(self, value):
        if self.root == None:
            self.root = Node(value)
        self.__r_insert(self.root, value)
```
위 코드는 이진트리의 삽입을 재귀적으로 하는 메소드다. 먼저 r_insert메소드가 시작점이 된다. r_insert메소드 안에 조건문을 보면 알 수 있듯이, 루트노드가 None, 즉 빈 이진트리에 삽입할 때는 root를 노드로 연결한다.