---
title: '[Doubly Linked List] Swap Nodes in Pairs 이중연결리스트 응용문제 파이썬으로 풀어보자'
author: baduk
date: 2023-12-21 15:29:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study,doubly linked list, python, 이중연결리스트]
---
## 문제소개
Swap Nodes in Pairs 문제는 이중으로 연결된 노드가 있는데, 각 노드들의 짝궁의 자리를 쭉 스왑하여 해결해야 하는 문제이다.

예를들면,

`1 <-> 2 <-> 3 <-> 4`
이와 같이 연결된 노드가 있다고 했을 때, 각 노드의 짝궁의 자리를 스왑하면,

`2 <-> 1 <-> 4 <-> 3`
이렇게 바뀌어져야 한다.

만약 노드의 개수가 `1 <-> 2 <-> 3 <-> 4 <-> 5`와 같이  홀수일 때는 `2 <-> 1 <-> 4 <-> 3 <-> 5`로 바뀔 수 있도록 하면된다.

## 해결과정
문제를 푸는과정동안 사실 너무 복잡하게 느껴지고 헷갈리기도 했다. 기본적인 아이디어는 노드를 하나씩 옮겨가면서 연결선을 바꿔주는식이다. 근데 그 과정이 너무 복잡했다.

내가 작성한 코드는 아래와 같다.
```python
    def swap_pairs(self):
        if self.length <= 1:
            return
        before = self.head
        temp = before.next
        after = temp.next
        while True:
            before.next = after
            temp.prev = before.prev
            if before.prev != None:
                before.prev.next = temp
            else:
                reserved_head = temp
            before.prev = temp
            if after != None:
                after.prev = before
                reserved_tail = after
            else:
                reserved_tail = before
            temp.next = before
            
            if after != None and after.next != None:
                before = after
                temp = before.next
                after = temp.next
            else:
                break
        self.head = reserved_head
        self.tail = reserved_tail
        return True
```
코드를 꽤 복잡하고 길게 작성하게 되었다. 먼저 노드의 개수가 1개 이하일 경우 바로 return하도록 하였다. 그리고 처음 head를 before변수에 저장하고, 그리고 순서대로 before다음 노드를 temp변수에 temp다음 노드를 after변수에 저장하였다.

그런다음 이 3개의 변수를 활용해서 노드연결을 바꿔주는 로직이 진행된다. 하지면 노드 연결을 바꿔주는 과정을 거칠 때, before 부분이 None일 수 있는 상황이 있어서, 10번라인에서 그부분을 조건문으로 처리하도록 하였다. None값이면 next로 연결할 수 없기 때문이다. 만약 None값이면 나중에 head가 될 여지가 있는 노드로 reserved_head라는 변수에 그 주소를 저장하도록 하였다. 그리고 그 다음 조건문(15번라인)에서는 after가 None일 수 있는 상황에 대비한 조건문이다. after가 None일경우 prev로 연결할 수 없으므로 그 부분에 대한 처리를 할 수 있도록 하였다. 그리고 tail노드가 될 여지가 있는 after 또는 before을 reserved_tail에 그 주소를 저장하도록 하였다. 그리고 마지막 조건문(22번라인)에서는 after가 끝에 다다르면 반복을 벗어나도록 하기위해 작성하였다.

이렇게 반복문이 끝나고나서 아까 저장해둔 head, tail이 될 여지가 있는 reserved_head, reserved_tail을 self.head와 self.tail이 각각 가리키도록하였다.


## 선생님이 작성한 코드
```python
    def swap_pairs(self):
        dummy_node = Node(0)
        dummy_node.next = self.head
        previous_node = dummy_node
    
        while self.head and self.head.next:
            first_node = self.head
            second_node = self.head.next
    
            previous_node.next = second_node
            first_node.next = second_node.next
            second_node.next = first_node
    
            second_node.prev = previous_node
            first_node.prev = second_node
    
            if first_node.next:
                first_node.next.prev = first_node
    
            self.head = first_node.next
            previous_node = first_node
    
        self.head = dummy_node.next
    
        if self.head:
            self.head.prev = None
```
선생님이 작성한 코드를 보면 Dummy Node를 추가해서 해결한 것을 볼 수 있다. 이렇게 하는 방식도 있기는 하겠지만, 솔직히 내가 작성한 코드나 선생님이 작성한 코드나 복잡한것은 마찬가지인듯 하다. 선생님이 작성한 코드와 내가 작성한 코드 차이를 보자면, 먼저 더미노드 사용여부가 있고, 반복문을 진행할 때 나는 무한루프 안에서 조건문으로 빠져나오지만, 선생님이 작성한 코드에서의 반복문은 head를 바꿔가면서 진행해 나간다. 그래도 내코드가 조금 더 온전하다 할 수 있을만한 부분은 나는 tail부분도 정확하게 가리키도록 하였다. 하지만 선생님도 문제에서 미리

```Note: This DoublyLinkedList does not have a tail pointer which will make the implementation easier.```

라고 하시긴하셨지만, 그래도 난 tail까지 가지고 갈 수 있도록 구현해보았다.