---
title: '[Python] 얕은복사, 깊은복사 개념 + 리스트 초기화시 주의할 것'
author: baduk
date: 2024-01-07 15:49:00 +0900
categories: [Study, Python]
tags: [Python, 파이썬, 파이썬 문법]
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

# 얕은복사, 깊은복사

## 리스트 그냥 복사했을때 코드 예시
```python
list_A = ["A",["D","C"]]
list_B = list_A

list_B[1][0] = '여기바뀜'
list_B[0] = '여기바뀜'

print(list_A, list_B)
```
먼저 위 코드가 어떻게 출력될지 예상해보자. 깊은복사와 얕은복사에 대해서 잘 모른다면, list_A에는 값이 바뀌지 않고 list_B만 바뀔 것이라고 생각할 수 있다. 하지만 list_B가 업데이트 되면서 list_A도 같이 업데이트 된다. 실제로는 리스트가 한개인데 둘다 같은 주소를 가리키고 있기 때문이다. 출력결과는 아래와 같다.

```python
#출력결과
['여기바뀜', ['여기바뀜', 'C']] ['여기바뀜', ['여기바뀜', 'C']]
```

<br>

---


## copy()를 사용해서 복사했을 때 코드 예시(얕은복사)
```python
list_A = ["A",["D","C"]]
list_B = list_A.copy()

list_B[1][0] = '여긴바뀜'
list_B[0] = '여기바뀜'

print(list_A, list_B)
```
위 코드처럼 copy()를 사용해서 리스트를 복사할 수 있다. 이 때는 얕은 복사가 일어난다. 얕은복사는 이름그대로 얕게 복사를 한다. 따라서 위 코드를 실행하면 list_A 자체는 복사되어서 list_B를 수정해도 바뀌지는 않는다. 하지만 list_A안에 있던 리스트(["D","C"])는 겉복사(?)가 이루어져서, list_A안에 있는 리스트를 수정하면 list_B안에 있는 리스트도 바뀌게된다. 출력결과는 아래와 같다.

```python
#출력결과
['A', ['여긴바뀜', 'C']] ['여기바뀜', ['여긴바뀜', 'C']]
```


<br>

---


## copy.deepcopy() 사용해서 복사했을 떄 코드 예시(깊은복사)
```python
import copy

list_A = ["A",["D","C"]]
list_B = copy.deepcopy(list_A)

list_B[1][0] = '여긴바뀜'
list_B[0] = '여기바뀜'

print(list_A, list_B)
```
위와 같이 깊은 복사를 해야 비로소 서로가 서로를 건드리는 일이 없어진다. 깊은복사를 할 때는 copy라이브러리를 불러와야 한다. 출력결과는 아래와 같다.

```python
#출력결과
['A', ['D', 'C']] ['여기바뀜', ['여긴바뀜', 'C']]
```

<br>

---
# 배열 초기화시 주의할 것

## 2차원 배열 초기화1
```python
arr = [[0]*2]*2
```
위 코드와 같이 입력하면 arr 안에는 `[[0, 0], [0, 0]]` <- 이와 같은 배열이 들어간다. 하지만 이렇게 하게되면, arr[0][0]을 1로 바꾸면 arr[1][0]에도 1이 들어가는 곤란한 문제가 발생한다.


## 2차원 배열 초기화2
```python
arr = [[0 for i in range(2)] for j in range(2)]
```
위 코드와 같이 작성하면 arr[0][0]을 1로 바꾸더라도 arr[1][0]에 영향을 주지 않는다.


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>
