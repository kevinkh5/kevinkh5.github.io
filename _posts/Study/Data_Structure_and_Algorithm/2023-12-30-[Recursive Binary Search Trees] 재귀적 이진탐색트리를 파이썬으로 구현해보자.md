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
이전에 이진트리의 insert는 위 코드와 같이 재귀를 활용하지 않고 구현했으나, 이 포스팅에서는 재귀를 활용하여 insert도 구현해볼것이다.

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
코드는 위와 같이 두개의 함수로 구성될 수 있고, 먼저 시작점으로서 r_contains가 불러와지고 그 다음 __r_contains메소드가 불려지는데, 재귀적 호출은 __r_contains메소드에서 이루어진다. 찾고자 하는 노드를 찾을때까지 혹은 current_node의 위치가 None으로 갈 때 까지(이때는 찾고자 하는 노드는 없는 경우) 계속해서 호출하다가 True 또는 False를 반환하게된다.

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
위 코드는 이진트리의 삽입을 재귀적으로 하는 메소드다. 먼저 r_insert메소드가 시작점이 된다. r_insert메소드 안에 조건문을 보면 알 수 있듯이, 루트노드가 None, 즉 빈 이진트리에 삽입할 때는 root를 새 노드에 연결한다.

그리고 4번, 6번라인에서 삽입할 노드의 위치를 탐색하고, current_node가 None이 되어서 맨 끝에 삽입할 위치에 도달하면 Node(value)를 반환하면서 새로삽입한 노드가 연결된다.

## delete 구현하기(재귀이용)
재귀를 활용한 이진트리에서 delete메소드는 막 어렵지는 않지만 몇가지 경우에 맞춰서 코드를 작성해야하기 떄문에, 다소 코드가 길다.
```python
    def __delete_node(self, current_node, value):
        if current_node == None:
            return None
        if value < current_node.value:
            current_node.left = self.__delete_node(current_node.left, value)
        elif value > current_node.value:
            current_node.right = self.__delete_node(current_node.right, value)
        else:
            if current_node.left == None and current_node.right == None:
                return None
            elif current_node.left == None:
                current_node = current_node.right
            elif current_node.right == None:
                current_node = current_node.left
            else:
                sub_tree_min = self.min_value(current_node.right)
                current_node.value = sub_tree_min
                current_node.right = self.__delete_node(current_node.right, sub_tree_min)
        return current_node
    
    def delete_node(self, value):
        self.__delete_node(self.root, value)
```
위 코드를 보면 delete_node 메소드를 시작점으로 delete를 진행한다. delete_node메소드 안에서 __delete_node를 부르고, __delete_node에서 재귀가 일어나면서 노드를 삭제한다. __delete_node내부에 작성된 코드를 설명하자면, 먼저 입력된 current_node가 None이 나왔다는 것은, 지우고자 하는 value가 존재하지 않아서이고, 따라서 그대로 None을 반환한다. 4번, 6번라인은 삭제하고자 하는 노드를 찾는 과정이다. 찾고자 하는 노드의 크기가 현재 노드보다 작으면 노드를 왼쪽으로 이동시키고, 크면 오른쪽으로 이동시켜나간다. 그러다가 만약 찾고자 하는 노드가 현재노드에 위치하게되면(즉, 지우고자하는 노드를 발견하면) 8번라인으로 오게 된다.

지우고자 하는 노드를 발견하고 나서는 4가지 경우를 마주하게 된다. 아래 사진 처럼 지우고자 하는 노드가 빨간색이라고 했을 때, 1번의 경우 지우고자 하는 노드 left, right 모두 None인 경우이고, 2번은 지우고자하는 노드의 오른쪽에만 노드가 있는 경우 3번은 왼쪽에만 노드가 있는 경우, 마지막 4번은 지우고자 하는 노드의 양쪽(left, right)에 모두 노드를 가진 경우다.

<img src = "https://lh3.googleusercontent.com/pw/ABLVV85HKbDENiQxcQiEumk_nrLsyHX3venfA-1xGhcIiYJX9symSXN5K8qhFfjj1cM88c8bQLn4zUThOzpzBaJ9DmqitopvV6TBNnytEaPRRiKiBEGWwnqrBeFOlSt3C8ONG05qKd76UBreNk413uBbWzg=w866-h655-s-no-gm?authuser=0" alt="exam" width="400" height="400">

1번의 경우 9번 라인으로 들어가서 None을 그대로 반환하면 되고, 2번과 같이 오른쪽에만 노드를 가진 경우는 11번 라인으로 들어가서 현재 노드를 지우고자 하는 오른쪽 노드로 바꿔서 최종적으로 지워질 노드의 오른쪽 노드가 지워진 노드자리로 오도록 연결시키면 된다. 3번은 2번과 반대로 현재 노드를 지워질 노드의 왼쪽 노드로 바꿔서 같은 방식으로 처리하면 된다.

이제 다소 복잡한 경우가 4번 처럼 노드를 둘다 가진 경우인데, 4번의 경우의 노드를 지울 때, 해당 노드를 지우고 나서 지워진 노드를 채울 다른 노드를 넣어야 한다. 그리고 그 채울 노드가 바로 `지울 노드의 오른쪽 노드를 root노드로 잡았을 때의 최소값을 가진 노드`가 된다.

즉, 다시말하자면 지울노드의 오른쪽 노드들 중에서 가장 작은 값이 지워질 노드를 채울 노드가 된다. 그 노드가 들어간다면 당연히 지워질 노드의 왼쪽 노드들 보다는 어쨌든 크기 때문에 지워질 노드들의 왼쪽 노드들의 배치는 건드릴 필요가 없다. 그리고 오른쪽 노드들 역시 마찬가지로 오른쪽 노드들에서 최소를 가져온 것이기 때문에 오른쪽 노드들의 배치를 건드릴 필요가 없다.

4번에 경우에는 최소값을 가진 노드를 구해야하기 때문에 아래 최소값을 구하는 메소드를 추가해야한다.

```python
    def min_value(self, current_node):
        while current_node.left is not None:
            current_node = current_node.left
        return current_node.value
```

이렇게 재귀를 활용한 이진트리를 파이썬 코드와 함께 구현해보았다.