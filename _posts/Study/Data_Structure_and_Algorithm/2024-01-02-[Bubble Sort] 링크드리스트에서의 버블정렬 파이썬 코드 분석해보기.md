---
title: '[Bubble Sort] 링크드리스트에서의 버블정렬 파이썬 코드 분석해보기'
author: baduk
date: 2024-01-02 13:10:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Bubble Sort, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 문제
버블정렬은 맨 왼쪽부터 두개씩 수를 비교하여 가장 큰 값 부터 맨 오른쪽으로 차례대로 밀어붙이는 식으로 정렬된다.

## 내가 작성한 코드
```python
    def bubble_sort(self):
        temp2 = self.tail
        while temp2 != self.head:
            temp = self.head
            while temp != temp2:
                if temp.value > temp.next.value:
                    temp.value, temp.next.value = temp.next.value, temp.value
                temp3 = temp
                temp = temp.next
            temp2 = temp3
```
나는 먼저 temp2에 tail 노드의 주소를 담았다. 그 이유는 버블정렬은 정렬처리가 진행 될 때 마다, 처음에는 맨 끝까지 체크가 들어가지만, 점차 맨 끝에서 부터 한칸씩 체크과정이 줄어들기 때문에 그 부분을 처리하기 위해 temp2라는 변수를 선언했다. temp에는 head의 주소를 담았는데, 그 이유는 버블정렬 처리과정에서의 시작점을 만들어주기 위함이다. 두번째 while문 안쪽에서는 스위치가 일어난다. temp3 변수를 만든 이유는 temp2에 한칸씩 줄어든 값을 넣기위해서이다. 정렬 마지막 과정에서는 temp3에는 head의 주소가 들어가고, head의 주소가 들어간 temp3는 temp2로 전달되니까 3번라인 while문의 조건으로 (temp2 != self.head)부분이 들어갔다.

 코드가 좀 난해하고 복잡하다.

## 또 다른 코드
```python
def bubble_sort(self):
    if self.length < 2:
        return

    sorted_until = None

    while sorted_until != self.head.next:
        current = self.head
        while current.next != sorted_until:
            next_node = current.next
            if current.value > next_node.value:
                current.value, next_node.value = next_node.value, current.value
            current = current.next
        sorted_until = current
```
일단 크기가 2보다 작으면, 즉 숫자의 개수가 1개나 0개이면 그냥 리턴해야 하므로 2~3번라인에서 그 부분을 처리하였다. sorted_unitl은 한번 정렬 과정이 지날 때마다, 맨 오른쪽이 정렬되기때문에, 정렬 과정을 한칸씩 줄도록하여 처리할 수 도록 하기위해 만들어준 것이다. 처음에는 next_node가 tail이 될 때까지 가겠지만, (즉 next_node가 None이 되기전 까지가겠지만) 이러한 과정이 지나면서 next_node가 앞으로 땡겨져야 한다. 왜냐하면 끝은 이미 정렬이 된 것이라서 스위치할 필요가 없기 때문이다. 그렇게 sorted_until에는 None으로 시작해서 tail, tail전 노드, tail전전 노드.. 이런식으로 차례로 들어갈 것이고, 그러다가 마지막 정렬과정에서 첫번째 인덱스와 두번째 인덱스까지 정렬을 마치면 sorted_until에는 self.head.next와 같은 주소가 들어갈 것이다. self.head.next와 같은 주소가 되면 정렬이 끝났으니까 반복을 그만 두어야 하므로 7번라인에서 조건을 sorted_until != self.head.next으로 한것이다.

## 후기
버블정렬이라는 아이디어가 어려운 것은 아니지만, 이것을 여러 반복문을 사용해서 처리해야 하다보니까 많이 헷갈린다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>