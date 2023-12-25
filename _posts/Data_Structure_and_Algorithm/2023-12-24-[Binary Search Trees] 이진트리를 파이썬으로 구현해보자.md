---
title: '[Binary Search Trees] 이진트리를 파이썬으로 구현해보자'
author: baduk
date: 2023-12-24 14:35:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Binary Search tree, 이진트리, python, problem solving, algorithm]
---
## Constructor를 구현해보자

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
    
class BinarySearchTree:
    def __init__(self):
        self.root = None
```

이진 트리의 각 노드는 차일드 노드를 총 두개 가질 수 있으므로, 위 코드에 작성된 Node class에서도 볼 수 있듯이 init에는 left, right가 있다. 또한 그 노드가 가지고 있는 value를 저장하기 위해 value변수가 포함되어있다. constructor는 이와 같이 간단하게 코드로 구현하면 된다.


## insert 구현하기

```python
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

이진트리의 insert메소드를 구현하는 것도 크게 어렵지는 않다. 기본적인 로직을 설명하자면, 먼저 삽입할 노드를 생성하고, 만약 root노드가 None이면(다른말로 말하면 노드가 비어있으면), root노드에 새로운 노드를 삽입하면 된다. 그리고 만약 root노드가 존재하면, 새로 삽입할 노드와 루트 노드의 value의 크기를 비교하면서 새로 입력할 노드의 위치를 탐색해 나가면 된다.

만약 새로입력할 노드의 value가 비교한 노드보다 크다면 오른쪽으로 이동하도록 하면되고, 작다면 왼쪽으로 이동하도록 하면된다.

이진트리에서는 같은 value를 가진 노드가 2개 이상 존재할 수 없으므로 위 코드 8번라인에서도 볼 수 있듯이, 새로입력할 노드의 value가 기존의 노드의 value와 같다면 삽입할 수 없으므로 False로 리턴하도록 처리하였다.

### contains 구현하기

contains메소드가 하는 일은 찾고하자 하는 노드가 있는지 없는지를 확인하는 것이다. 만약 내가 찾고자하는 노드가 트리에 있으면 True를 반환하고 없으면 False를 반환할 것이다. 코드는 아래와 같다.

```python
    def contains(self, value):
        if self.root == None:
            return False
        temp = self.root
        while temp != None:
            if temp.value == value:
                return True
            elif temp.value > value:
                temp = temp.left
            else:
                temp = temp.right
        return False
```
먼저 루트노드가 존재하지 않다면 그대로 False를 반환하면 되고, 루토노드가 존재한다면, 찾고자 하는 value를 가진 노드를 탐색해 나가면된다. insert메소드를 구현할 때와 마찬가지로 찾고자하는 value가 작으면 왼쪽으로, 크다면 오른쪽으로 이동해가면서 찾아나가면 된다. 만약 찾고자 하는 노드가 없으면 while문을 벗어나면서 False를 반환할 것이고, 있으면 6번라인(if temp.value == value:)이 실행되면서 True를 반환하게 될 것이다.

지금까지 이진트리를 파이썬으로 구현해보았다. insert메소드와 contains메소드 두개 모두 크게 복잡하지 않아서 구현하는데 크게 어려움은 없었다.

## 이진트리 vs 연결리스트
추가적으로 알면 좋은것이 있는데, 이진트리는 데이터를 탐색할때 시간복잡도가 O(n)인 연결리스트에 비해서 시간복잡도가 일반적으로 O(logn)으로 굉장히 빠르다.
(이진트리의 시간복잡도가 O(logn)이 나오는 이유가 궁금하다면? [여기클릭!](https://kevinkh5.github.io/posts/Binary-Search-Trees-%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC-%ED%83%90%EC%83%89%EC%9D%98-%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84%EA%B0%80-O(logN)%EC%9D%B8-%EC%9D%B4%EC%9C%A0/))

하지만 삽입 과정에서 연결리스트의 시간 복잡도는 O(1)이고, 이진트리에서는 O(logn)다. 따라서 삽입과정은 연결리스트가 훨씬 빠르다.
(삽입과정에서 이진트리는 best case, average case 모두 O(logn)이고  worst case는 O(n)으로 항상 O(logn)인것은 아니다. 빅오는 worst case로서 정해지므로 이진트리 삽입의 시간복잡도는 technically O(n)이다.)

따라서 검색작업의 빈도가 많고 검색속도가 중요한 경우, 이진트리를 쓰는 것이 좋을 것이고, 검색보다는 데이터 저장속도가 중요한 경우 연결리스트를 쓰면 좋을 것이다.

```
속도비교
search: 연결리스트 < 이진트리
insert: 연결리스트 > 이진트리
```