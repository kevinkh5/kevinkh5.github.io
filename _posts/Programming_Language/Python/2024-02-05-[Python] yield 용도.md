---
title: '[Python] yield 용도'
author: baduk
date: 2024-02-05 07:14:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬, 파이썬 라이브러리]
---
## yield vs return
yield는 return과 비슷한 역할을 하지만 yield를 포함한 함수는 return을 포함한 함수와 동작과정과 그 특징이 많이 다르다.

아래 return을 포함한 함수를 fun1(3)으로 실행하면, a를 출력하고 i를 리턴 받는다.
```python
def fun1(num):
    for i in range(num):
        print('a')
        return i
fun1(3)
# 출력
# a
# 0(리턴 값)
```

하지만 yield를 포함한 함수는 어떨까? 아래 함수를 실행하면 b를 출력하고 i를 리턴받을 것 같지만, 아래와 같이 generator object라는 값을 리턴받는다.
```python
def fun2(num):
    print('yield가 있어요')
    for i in range(num):
        print('b')
        yield i

fun2(3)
# 출력
# <generator object fun2 at 0x7fb620855820>
```

이와 같이 def 아래 yield가 있으면, 파이썬은 이것을 함수가 아니라 generator 취급을 한다. 따라서 yield가 붙은 def는 for 반복문에서 꺼내오는 식으로 종종 사용된다. 그리고 yield가 포함된 함수를 반복문에서 하나씩 실행하는데, 실행과정이 return과는 차이가있다. 아래 코드를 한번 확인해 보자.

```python
def fun2(num):
    print('yield가 있어요')
    for i in range(num):
        print('a')
        yield i
        print('b')
        yield i
        print('c')
        yield i

for i in fun2(2):
    print('-----')
    print(i)
    print('-----')
# 출력
# yield가 있어요
# a
# -----
# 0
# -----
# b
# -----
# 0
# -----
# c
# -----
# 0
# -----
# a
# -----
# 1
# -----
# b
# -----
# 1
# -----
# c
# -----
# 1
# -----
```

return이 포함된 def에서는 함수를 실행할때마다 처음부터 시작을 하지만 yield가 포함된 def를 실행하면 처음 실행에서는 처음부터 시작을 하지만 그다음 부터는 시작포인트가 yield지점이후가 된다.

이 처럼 yield를 쓰면 함수를 제너레이터 처럼 쓸 수 있는데 이게 좋은점이
for반복문안에서 함수의 리턴값을 받으면서 처리하기 좋다. 그리고 리턴하더라도 그 시점을 저장하고 있어서 효율적인 처리가 가능하다.

## yield 사용사례

아래 코드는 프로그래머스 문제 중 하나 인 단어변환이라는 문제를 풀 때 사용된 코드로, 코드안에서 yield가 활용되었다.
```python
from collections import deque


def get_adjacent(current, words):
    for word in words:
        if len(current) != len(word):
            continue

        count = 0
        for c, w in zip(current, word):
            if c != w:
                count += 1
                if count > 1:
                    break

        if count == 1:
            yield word


def solution(begin, target, words):
    dist = {begin: 0}
    queue = deque([begin])

    while queue:
        current = queue.popleft()

        for next_word in get_adjacent(current, words):
            if next_word not in dist:
                dist[next_word] = dist[current] + 1
                queue.append(next_word)

    return dist.get(target, 0)

print(solution("hit","cog",["hot", "dot", "dog", "lot", "log", "cog"]))
```
yield를 사용함으로써 get_adjacent함수를 제너레이터로 바꾸어주었고, 이 함수를 반복문에 사용한다. 이때 현재 비교할 단어와 단어리스트들을 하나씩 꺼내면서 비교하다가 알파벳 한글자만 다른 단어를 발견하면 그 단어를 next_word로 넘겨주고, 현재 단어가 가진 값에 +1을해서 그 단어를 딕셔너리에 저장 시킨다. 그리고 다시 get_adjacent를 반복하는데 이때, 제너레이터인 get_adjacent는 실행할 때 처음부터 다시 실행되는게 아니라 아까 yield로 반환한 단어의 그 다음단어부터 검사를 시작한다. 이 부분이 굉장히 효율적이다. 

yield를 이용한 제너레이터는 메모리를 효율적으로 사용하면서도 큰 데이터 세트를 처리하는 데 도움이된다고 한다. yield를 이용한 처리방식이 다소 독특하지만, 이와같이 적절한 상황에서 잘 쓰면 효율적이고 간결한 코드를 작성할 수 있다. 