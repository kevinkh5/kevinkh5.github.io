---
title: '[Dynamic Programming] 동적계획법을 피보나치 함수로 간단하게 이해해보자'
author: baduk
date: 2024-01-05 14:07:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Dynamic Programming, DP, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 동적계획법
동적계획법은 큰 문제를 작게 나누고, 그 작은 문제들을 해결해 나가면서 큰 문제를 해결해나가는 알고리즘 기법이다. 
작은 문제들을 해결하면서 구한 정답은 특정 저장공간에 넣어두었다가, 큰 문제를 해결할 때 필요한 정답을 그 저장공간에서 다시 꺼내서 재사용한다. 이를 메모이제이션(Memoization)기법이라고 한다.

## 간단한 코드로 구현한 피보나치 함수
```python
def fibo(n):
    if n==0:
        return 0
    elif n==1:
        return 1
    return fibo(n-2) + fibo(n-1)
```
피보나치 함수를 코드로 단순하게 작성하자면 위와 같이 재귀적인 방법을 사용하도록 함수를 구현할 수 있을 것이다. 하지만 위와 같이 피보나치 함수를 작성하면 피보나치 함수가 입력받는 n의 크기가 크면 클 수록 계산해야할 양이 기하급수적으로 늘어날 것이다. 이 과정에서 이미 구했던 중복된 값도 여러번 계산하기 때문에 굉장히 비효율 적이게 된다.

따라서 아래 코드와 같이 이미 계산된 값은 메모리공간에 따로 저장해두었다가, 필요할 때 꺼내쓸 수 있도록 하는 아이디어를 적용시키면 훨씬 더 계산이 효율적이게 될 것이다.
## 효율적으로 피보나치 함수 구현하기1
```python
def fibo2(n):
    if n in memoization:
        return memoization[n]
    else:
        if n==0:
            memoization[n] = 0
            return 0
        elif n==1:
            memoization[n] = 1
            return 1
        memoization[n] = fibo2(n-2)+fibo2(n-1)
        return fibo2(n-2) + fibo2(n-1)
memoization = {}
#fibo2(5942)
```
위 코드는 memoization이라는 딕셔너리를 만들고 저장된 값이 없으면 memoization이라는 딕셔너리 안에 값을 넣고, 그 값이 필요할 때 불러서 쓸 수 있도록 작성하였다. 하지만 이것역시 재귀적인 방법을 사용하여, 수가 커지면 맥시멈 리컬젼 초과가 나면서 에러가 날 수 있다. 따라서 이를 피하기위해 아래와 같은 코드로 작성해볼 수도 있을 것 같다.

## 효율적으로 피보나치 함수 구현하기2
```python
def fibo3(n):
    memoization = [0 for i in range(n + 1)]
    memoization[0] = 0
    memoization[1] = 1
    
    for i in range(2, n + 1):
        memoization[i] = memoization[i-2] + memoization[i-1]
    return memoization[n]
```
위 코드는 `효율적으로 피보나치 함수 구현하기1`에서 작성했던 코드와 달리 재귀적인 방법이 사용되지 않는다. 그저 작은 수 부터 하나씩 차근차근 정답을 구하여 memoization 리스트에 저장해나간다. 이와 같이 코드를 구현하면 재귀적인 방법을 사용하지 않기 때문에 수가 커져도 맥시멈 리컬젼 초과에러가 발생하지는 않는다.



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>