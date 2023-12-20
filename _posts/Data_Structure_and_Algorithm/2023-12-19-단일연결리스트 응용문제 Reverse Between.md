---
title: '[Linked List] 응용문제 Reverse Between'
author: baduk
date: 2023-12-19 16:41:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, linked list]
---
Reverse Between 문제는 단일연결리스트로 연결된 노드가 있을때, 특정 인덱스 구간의 노드의 순서를 거꾸로 바꾸는 문제다.

예를들어, 
`1 -> 2 -> 3 -> 4 -> 5` 로 이루어진 단일연결된 노드가 있다. 만약 start_index = 1이고, end_index가 3이면, 이 노드에서 1 -> `2 -> 3 -> 4` -> 5 색칠된 부분인 `2 -> 3 -> 4`를 `4 -> 3 -> 2`로 바꾸어야 한다. 따라서 결과적으로 이 노드는 `1 -> 4 -> 3 -> 2 -> 5`가 되어야 한다.

문제는 이 문제를 풀기는 했지만, 나랑 답이 좀 달랐다. 그리고 모범답안에 있는 답이 정말 이해하기 어려웠다. 어떻게 이러한 아이디어를 떠올릴 수 있는지 솔직히 납득이 안간다.

모범답안은 아래와 같다.
```pythons
    def reverse_between(self, start_index, end_index):
        if self.length <= 1:
            return
    
        dummy_node = Node(0)
        dummy_node.next = self.head
        previous_node = dummy_node
    
        for i in range(start_index):
            previous_node = previous_node.next
    
        current_node = previous_node.next
    
        for i in range(end_index - start_index):
            node_to_move = current_node.next
            current_node.next = node_to_move.next
            node_to_move.next = previous_node.next
            previous_node.next = node_to_move
    
        self.head = dummy_node.next
```
12번라인까지는 오케이 이해간다. 근데 14번라인 for문 돌리고 있는 쪽이 정말 어렵다. 

14번라인 부터는 아이디어가 3개의 포인터를 기본적으로 이용하면서, node_to_move를 앞쪽 start_index자리로 끌어오고 밀어내고 다시 node_to_move를 앞으로 또 끌어오는것을 end_index - start_index 만큼, 즉 그 GAP횟수만큼 반복시켜서 결과적으로 특정 인덱스 구간의 노드순서를 역순으로 정렬해나가는 방식이다.

애니메이션을 통해 보자면 아래와 같다.
![gif](https://lh3.googleusercontent.com/pw/ABLVV84ihsOQid9fW-7VM3BBZoOwQ_DDfZ_HqlDWVYRc0V3xDulaEcG7cSqxDg51QE3yz7g3K9PvOcSQoqGmMfmpC7g5-wde669FGX8QMt1yMJsQWIGDS_a4kMkssBQ0sy13ZAmLqnLOyJg4wcT-4gkq9Ww=w940-h788-s-no-gm?authuser=0)

이건 코드로 구현하기 이전에 이러한 아이디어를 떠올리는것은 정말 쉽지 않은 일인것 같다.

일단 코드 자체를 외우는 방식으로 접근하기보다는 이 아이디어가 있다는 사실을 머리에 새기고 넘어가는걸로.😂