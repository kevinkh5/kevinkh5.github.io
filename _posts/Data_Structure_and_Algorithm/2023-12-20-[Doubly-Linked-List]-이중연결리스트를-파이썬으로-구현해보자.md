---
title: '[Doubly Linked List] 이중연결리스트를 파이썬으로 구현해보자'
author: baduk
date: 2023-12-20 17:58:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study,doubly linked list, python]
---
이중연결리스트를 그림으로 표현하면 다음과 같다

![photo](https://lh3.googleusercontent.com/pw/ABLVV856S-j7ixrf3-4lOik9eyri_NrrLsI6BwAWlSjicVOv0c8CmWerHvI3_wX0hJkj-BOopcSct0WVnJcAPbOlCsLCabPbsPWK40KyLSnXuF45BRtWOe9CV_30SdJWrzIdIua0rBt10veAi4RpaqZImG4=w940-h400-s-no-gm?authuser=0)

이 그림대로 노드가 연결되어있다고 생각하고 구현해나가면 된다. 실수로 노드 두개 모두 1을 중복해서 넣었는데 그건 일단 무시하자...

---
### constructor 구현하기

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

---
### prepend 구현하기(맨 왼쪽 노드로 append)
```python
    def prepend(self,value):
        new_node = Node(value)
        if self.length == 0:
            self.head = new_node
            self.tail = new_node
        else:
            self.head.prev = new_node
            new_node.next = self.head
            self.head = new_node
        self.length += 1
        return True
```
prepend도 append와 구현방식은 같으므로 설명은 생략하겠다.

---
### pop frist 구현하기(맨 왼쪽 노드에서 pop)
```python
    def pop_first(self):
        if self.length == 0:
            return None
        temp = self.head
        if self.length == 1:
            self.head = None
            self.tail = None 
        else:
            self.head = self.head.next
            self.head.prev = None
            temp.next = None
        self.length -= 1
        return temp
```
pop_first도 tail쪽 부터 pop하는 방식과 마찬가지로 구현하면 된다. pop_first는 head쪽에서 pop해주는것으로 반영시켜서 구현하면 쉽게 작성할 수 있다

---

### get 구현하기
```python
    def get(self, index):
        if index < 0 or index >= self.length:
            return None
        if index < self.length/2:
            temp = self.head
            for _ in range(index):
                temp = temp.next
        else:
            temp = self.tail
            for _ in range(self.length - 1, index, - 1):
                temp = temp.prev
        return temp
```
이중연결리스트에서 get메소드는 단일연결리스트보다 성능이 더 좋다. 단일연결리스트에서는 오직 맨 왼쪽노드(head부분)부터 시작해서 인덱스를 찾아가는 방식이지만, 이중연결리스트에서는 get하고자 하는 index가 왼쪽에서 가까운지 오른쪽에서 가까운지를 먼저 판단하고 시작포인트를 왼쪽(head부분)부터 잡을 지 오른쪽(tail)부분부터 잡을 지를 결정한다. 따라서 이중연결리스트에서 get이 단일연결리스트보다 훨씬 빠를 것이다. 이와같이 이중연결리스트의 장점을 활용할 수 있게 코드를 구현하면 된다.

---
### set 구현하기
```python
    def set_value(self, index, value):
        temp = self.get(index)
        if temp:
            temp.value = value
            return True
        return False
```

set구현은 위에서 구현했던 get을 활용하면 간단한 코드로 구현 할 수 있다.

---

### insert 구현하기
```python
    def insert(self, index, value):
        if index < 0 or index > self.length:
            return False
        if index == 0:
            return self.prepend(value)
        elif index == self.length:
            return self.append(value)
        else:
            new_node = Node(value)
            before = self.get(index-1)
            after = self.get(index)
            new_node.prev = before
            new_node.next = after
            before.next = new_node
            after.prev = new_node
            self.length += 1
            return True
```
이중연결리스트의 insert구현도 단일연결리스트에서의 insert메소드 구현과 거의 흡사하다. 연결을 이중으로 해준다는 차이만 있을 뿐이다.

---
### remove 구현하기
```python
    def remove(self, index):
        if index < 0 or index >= self.length:
            return None
        if index == 0:
            return self.pop_first()
        if index == self.length - 1:
            return self.pop()
        temp = self.get(index)
        temp.next.prev = temp.prev
        temp.prev.next = temp.next
        temp.next = None
        temp.prev = None
        self.length -= 1
        return temp
```
remove도 위 insert와 비슷한 방식으로 구현해 볼 수 있다. get을 활용하고 이중으로 연결하면 된다.

---

여기까지 이중연결리스트를 파이썬으로 구현해보았다. 오히려 단일연결리스트보다 직관적으로 이해가 쉽고 코드작성이 훨씬 간단한 느낌이 들었다. 다만 연결을 이중으로 해야하다보니 코드가 조금 더 길어지는 느낌도 있다.