---
title: '[Python] 딕셔너리 정렬, sorted, lambda 사용하여 딕셔너리 값을 기준으로 정렬하기'
author: baduk
date: 2024-01-01 15:43:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬]
---
다음과 같은 딕셔너리가 있을 떄

```python
dic = {'a':10, 'b':20, 'c':30}
```

이 딕셔너리를 그 해당 값을 기준으로 아래와 같이 정렬하고 싶을 떄가 있다.

```python
dic_sorted1 = ['a', 'b', 'c'] # ascending 정렬
dic_sorted2 = ['c', 'b', 'a'] # desending 정렬
```

이럴 때는 sorted, lambda를 아래와 같이 활용하면 위와같이 정렬이 가능하다.

```python
dic_sorted1 = sorted(dic, key = lambda x: dic[x], reverse = False) # ascending 정렬
dic_sorted2 = sorted(dic, key = lambda x: dic[x], reverse = True) # desending 정렬
```