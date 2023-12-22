---
title: '[Stack] 스택을 파이썬 코드로 구현해보자'
author: baduk
date: 2023-12-22 14:08:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Stack, python]
---
## 스택소개
스택은 무언가를 담거나 뺄 수 있으며 입구가 `하나`인 통이라고 생각하면 이해가 쉬울 것 같다.

또한 스택은 마지막에 들어간 것을 가장먼저 뺄 수 있다.

영어로는 
Last In First Out => LIFO
라고 표현하기도 한다.

---

## Constructor 구현하기

먼저 파이썬으로 기초적인 뼈대를 코드로 구현해보자면 아래와 같다.
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
class Stack:
    def __init__(self, value):
        new_node = Node(value)
        self.top = new_node
        self.height = 1
        
    def print_stack(self):
        temp = self.top
        while temp is not None:
            print(temp.value)
            temp = temp.next
```

연결리스트([참고](https://kevinkh5.github.io/posts/Doubly-Linked-List-%EC%9D%B4%EC%A4%91%EC%97%B0%EA%B2%B0%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A5%BC-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9C%BC%EB%A1%9C-%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EC%9E%90/#constructor-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)) 구현할 때와 비슷한 방식로 class Node를 따로 작성하고 class stack을 작성하면 된다. 이중연결리스트의 노드의 경우 양방향으로 왔다갔다 해야하기 때문에, init값으로 next와 prev 두개를 두었었다. 하지만 스택은 하나의 구멍으로 주거니 받거니 하기 때문에 Node의 init값은 next하나만 있으면 된다. print_stack 메소드 부분도 연결리스트 구현과 거의 같은 방식이다.

---

## push 구현하기

push를 구현하는 것 역시 크게 어렵지 않다. 아래 그림을 보고 이해하면 쉽게 구현할 수 있다. 1이 먼저 들어가 있다고 했었을 때, 2가 push되면 최종적으로 그림과 같이 top이 마지막으로 들어간 노드를 가리키게 된다.
<img src = "https://lh3.googleusercontent.com/pw/ABLVV87MAogshOTEtbuZ02k2443nxRVfWM-fKgGCbEDS5bmeFrYMC7bZUgBYe2XLNA9eBrUdW30Vm7nZcKhbxYyPfbpii9r3SOcJEyQcU3DlnN0RNP73ACuXpfmNqb3KesJ7Kjm1S6aP8lqurjAX5NHbt_I=w780-h768-s-no-gm?authuser=0" width="400" height="400">

아래 코드에서도 볼 수 있듯, 만약 노드가 비어있는 stack에 push를 한다면 그냥 top이 새로운 노드를 가리키게 하면된다.

 하지만 만약 노드가 1개 이상 존재하는 stack에 push를 한다면, 먼저 새로운 노드의 next를 기존 top에 있던 노드에 연결후 top을 다시 새롭게 새로운 노드에 연결시키면 간단하게 구현가능하다.

 push메소드에 대한 코드는 아래와 같다.
```python
    def push(self, value):
        new_node = Node(value)
        if self.height == 0:
            self.top = new_node
        else:
            new_node.next = self.top
            self.top = new_node
        self.height += 1
```
---

## pop 구현하기

pop 구현도 어렵지 않게 작성할 수 있다. 코드를 보기전에 앞서 기본적인 원리를 설명하자면 top이 가리키는 방향을 이전노드로 바꾸어주고, 원래 top이 가리켰던 노드를 pop시키면 된다. 코드는 아래와 같이 간단하다.

```python
    def pop(self):
        if self.height == 0:
            return None
        else:
            temp = self.top
            self.top = temp.next
            temp.next = None
            self.height -= 1
            return temp
```

이상으로 stack을 파이썬 코드로 구현해 보았다.