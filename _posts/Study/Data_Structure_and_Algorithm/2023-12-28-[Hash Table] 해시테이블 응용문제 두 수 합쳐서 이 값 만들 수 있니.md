---
title: '[Hash Table] 해시테이블 응용문제, 두 수 합쳐서 이 값 만들 수 있니?'
author: baduk
date: 2023-12-28 09:47:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Hash Table, 해시테이블, python, problem solving, algorithm]
use_math: true
---
## 문제
숫자가 나열된 1 차원 리스트와 정수(target number)를 각각 총 2개를 입력 받는다. 1차원 리스트에서 숫자 두개를 뽑아서 합했을때 그 정수의 값을 만들 수 있느냐 하는 문제다. 있다면 그 위치를 출력해야 한다.

## 아이디어 떠올리기
이런 문제는 흔한 problem solving 문제인데, 여기서 두개의 숫자를 뽑는다 했을 때 자연스럽게 for문을 두개 쓰든지 해서 풀 생각을 하게 될텐데, 만약 for문을 두개 중첩해서 쓰면 시간복잡도가 $O(n^2)$이 되어 너무 느려진다. 이거를 $O(n)$으로 해결할 수 있는 방법이 있는데, 그게 바로 해시테이블 아이디어로 푸는 것이다. 파이썬에서는 딕셔너리를 사용하면 될 것이다.

그렇다면 도대체 딕셔너리를 써서 어떻게 해결해야 할까? 결론적으로는 해시테이블 방법을 쓰면 놀랍게도 루프를 딱 한번만 돌려서 끝낼 수 있다. 그 아이디어를 설명해보자면, 먼저 입력받은 리스트에서 숫자를 꺼내오고, 그 수를 정수(target number)에서 빼준다. 그리고 그 뺀 값을 딕셔너리에서 찾고, 만약 없으면, 꺼내온 숫자를 key로 넣고 value에는 그 수의 인덱스(위치)를 넣어 딕셔너리에 집어넣는다. 이런식으로 루프를 돌리면 된다.

이게 굉장히 빠를 수 있는 이유가, 뺀 값을 딕셔너리에서 찾을 때 하나 씩 루프를 돌려가면서 찾는 것이 아니라 key값을 가지고 검색을 하는 방식이라서 굉장히 빠르게 처리가 되는 것이다.

## 내가 제출한 코드
반복문을 중첩해서 작성하는 아이디어는 간단하지만 너무 느리다. 아래 코드는 어떻게든 해시테이블의 개념을 사용해서 풀려고 노력하면서 작성한 코드다. 
```python
def two_sum(nums, target):
    num_map = {}
    for i,_ in enumerate(nums):
        num_map[_] = i
        
    for i,_ in enumerate(nums):
        if target - _ in num_map and (i != num_map[target-_]):
            return [i, num_map[target-_]]
```
이렇게 작성하면 시간복잡도가 $O(n^2)$보다는 작겠지만 그래도 뭔가 복잡하고, 깔끔하지가 않은것 같다. 부족함이 많아 보이는 코드다. 첫 반복문에서 빈 딕셔너리를 채워주었고, 두번째 반복문에서는 채워진 딕셔너리에서 찾고자 하는 수를 검색하여 찾아가도록 로직을 만들었다. 두번째 반복문의 조건에서 같은 위치의 값을 리턴하지 않도록 처리하려다보니 조건문(7번 라인)도 좀 지저분해졌다.

## 모범 코드
```python
def two_sum(nums, target):
    num_map = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i
    return []
```
위 코드가 모범코드인데, 확실히 겉보기에도 반복문 하나로 깔끔하게 작성되었다. 내가 제출한 코드와 차이는 먼저 빈 딕셔너리를 미리 채우는 것이 아니라, for문을 돌면서 그때 그때 채우는 방식이다. 이렇게 했을 때, 루프를 정말 한번이상 돌지 않고 온전한 $O(n)$이 될 것이다.