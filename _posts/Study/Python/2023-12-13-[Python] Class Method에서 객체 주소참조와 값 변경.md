---
title: '[Python] Class Method에서 객체 주소참조와 값 변경'
author: baduk
date: 2023-12-15 18:52:00 +0900
categories: [Study, Python]
tags: [Python]
---
파이썬 코드로 연결리스트를 구현하는 과정에서 조금 이해가 안가는 부분이 있었다.
```python
    def remove(self, index):
        if index < 0 or index >= self.length:
            return False
        if index == self.length - 1:
            return self.pop(index)
        if index == 0:
            return self.prepop()
        prev = self.get_value(index-1)
        temp = self.get_value(index)
        prev.next = temp.next
        temp.next = None
        self.length -= 1
        return temp         
```
위 코드 8번라인에서 prev라는 변수에 Node객체를 넣고 10번라인에서 prev의 next를 변경했는데, 이게 내부적으로 값이 변경된다는 부분이 처음에는 잘 이해가 안갔었다. 하지만 주소에 접근해서 그 해당 주소가 가지고 있는 값을 변경한다는 점을 이해하면서 이 부분이 왜 그런지 납득되었다. temp와 같은 변수에 그 해당 주소를 가져오고, 그 temp를 이용해서 주소에 포함된 값을 변경하면 당연히 실제 주소접근이 되어서 변경이 된 것 이기 때문에 그 주소에 포함된 값이 바뀐다.

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
    
class LinkedList:
    def __init__(self, value):
        new_node = Node(value)
        self.head = new_node
        self.tail = new_node
        self.length = 1
```
링크드 리스트를 파이썬 코드로 구현하면 위와같이 Node라는 Class와 LikedList라는 Class를 두개 사용하는데 LinkedList Class내에서 Node라는 Class의 객체를 생성하고 그 해당 객체의 주소에 접근하면서 값을 변경하기 때문에, 그 해당 주소로 접근해서 값을 변경하면 그 해당 주소로 다시 접근했을 때 바뀐 값을 얻을 수 있는 것이다.

---
### 아래 코드를 보면서도 이해를 해볼 수 있는데,
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None
    
class MyClass:
    def __init__(self, value):
        node = Node(value)
        self.x = node  # 인스턴스 변수 x를 초기화
        
    def update_x(self, new_value):
        temp = self.x  # 현재 x의 값을 temp에 저장
        temp.value = new_value  # temp의 값을 변경
#         self.x = temp  # self.x에 temp 값을 할당하여 인스턴스 변수 x를 변경

# MyClass의 인스턴스를 생성
obj = MyClass(5)
print(obj.x.value)
print(obj.x)# 출력: 5

obj.update_x(3)
print(obj.x.value)
print(obj.x)
```

```
# Output
5
<__main__.Node object at 0x0000027000496340>
3
<__main__.Node object at 0x0000027000496340>
```
위 코드 12번라인부터 보면 temp에 self.x에 주소를 넣어서 그 해당하는 주소로 접근하여 value를 변경하고 있다. 출력 결과를 확인해 보면 value가 5에서 3을 변경된것을 확인할 수 있다. 하지만 주소는 그대로 '0x0000027000496340'로 유지되어 있는 것을 볼 수 있다.