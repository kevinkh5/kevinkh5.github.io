---
title: '[Python] *연산자, zip 사용과 전치행렬 만들기'
author: baduk
date: 2024-03-16 14:24:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬, 파이썬 라이브러리]
---

## '*' 연산자 용도
'*' 연산자는 언패킹 할 때 사용한다. 즉, 안에 요소들을 풀어헤칠 때 사용한다.

```python
#사용예시
print(*[1,2])
#출력
#1 2

print(*[[1,2],[3,4]])
#출력
#[1, 2] [3, 4]
```

## zip() 함수의 용도
zip()함수는 iterable한 즉, 반복가능한 객체를 인자로 받아 객체에서 `동일한 인덱스`에 위치한 요소를 튜플 형태로 묶어 반환한다.

```python
#사용예시
for e in zip([1,2],[3,4]):
    print(e)
#출력
#(1, 3)
#(2, 4)

for e in zip((1,2),(3,4)): # 물론 이렇게 튜플로 묶어도 결과는 같다.
    print(e)
```

zip만 쓰면 iterator(반복자)만 반환되기 때문에 list를 씌워 리스트로 반환할 수 있다.

```python
a = zip((1,2),(3,4))
print(a)
# 이터레이터로 반환됨
#<zip object at 0x>

print(list(a))
#리스트 씌우면 아래와 같이 출력
#[(1, 3), (2, 4)]
```

## '*' 연산자와 zip함수를 사용하여 전치행렬 구하는 간단한코드 작성해보기

아래 코드에서 행렬 arr1을 뒤집어 arr2(arr1의 전치행렬 형태)로 만들어보자.

```python
arr1 = [[1,2,3],[4,5,6],[7,8,9]]
arr2 = [[1,4,7],[2,5,8],[3,6,9]]

arr1_t = list(zip(*arr1))
# 출력
# [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
# 이렇게 출력하면 리스트 안에 튜플이 들어간 형태로 들어감
# 따라서 아래와 같이 map 사용해서 list로 바꾼형태로 넣어주기
arr1_t = list(map(list,zip(*arr1)))

print(arr1_t == arr2)
#  True
```