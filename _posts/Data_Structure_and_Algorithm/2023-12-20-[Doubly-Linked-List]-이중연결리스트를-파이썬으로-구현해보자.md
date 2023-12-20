---
title: '[Doubly Linked List] 이중연결리스트를 파이썬으로 구현해보자'
author: baduk
date: 2023-12-20 17:58:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study,doubly linked list]
---
이중연결리스트를 그림으로 표현하면 다음과 같다

![photo](https://lh3.googleusercontent.com/pw/ABLVV856S-j7ixrf3-4lOik9eyri_NrrLsI6BwAWlSjicVOv0c8CmWerHvI3_wX0hJkj-BOopcSct0WVnJcAPbOlCsLCabPbsPWK40KyLSnXuF45BRtWOe9CV_30SdJWrzIdIua0rBt10veAi4RpaqZImG4=w940-h400-s-no-gm?authuser=0)

이 그림대로 구현해나가면 된다.

---
### 기본세팅

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
        self.prev = None
    

class DoublyLikedList:
    def __init__(self, value):
        new_node = Node(value)
        self.head = new_node
        self.tail = new_node
        self.length = 1
    
    def print_list(self):
        temp = self.head
        while temp is not None:
            print(temp.value)
            temp = temp.next
```
일단 먼저 위 코드와 같이 Node에 대한 클래스를 먼저 세팅해주고, 이중연결리스트 클래스를 세팅해준다. 단일연결리스트와 다른점이 있다면 Node에 대한 클래스에 prev가 추가가 된다. 또한 연결리스트에 어떠한 값을 가진 노드가 연결되어있는지 확인해주기 위해서 print_list매소드를 작성해준다.

기본셋팅이 되었으면 그리고나서 연결리스트에서 사용할 몇가지 기능을 구현해보자.
---
### append 구현하기
```python
    def append(self, value):
        new_node = Node(value)
        if self.head is None:
            self.head = new_node
            self.tail = new_node
        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node
        self.length += 1
        return True
```
append도 단일연결리스트와 비슷하지만 차이가 있다. 이중연결리스트는 새로운 노드의 prev를 원래 tail 부분에 있었던 노드와 연결시켜주는 부분이 추가가 되었다.

---
### pop 구현하기

```python
    def pop(self):
        if self.length == 0:
            return None
        temp = self.tail
        if self.length == 1:
            self.head = None
            self.tail = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
            temp.prev = None
        self.length -= 1
        return temp
```
pop 구현도 비교적 간단하다. 먼저 노드가 없을경우 None을 리턴하게 만들어주고, 노드 한개일 경우 head와 tail을 모두 None에 연결하고 해당노드를 리턴해준다. 만약 노드가 두개 이상일 경우, tail을 앞으로 옮겨주고, 그 해당 tail의 next를 None에 연결하여 끊어준다. 그리고 temp에 저장해두었던 이전tail의 prev를 None을 넣어 끊어주고 해당 노드를 리턴해주면서 마무리하면된다.