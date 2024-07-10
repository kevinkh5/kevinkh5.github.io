---
title: '[Python] collections 라이브러리 Counter, 리스트에 특정 값을 딕셔너리에 key로넣고 value의 개수 넣기'
author: baduk
date: 2024-01-19 10:22:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬, 파이썬 라이브러리]
---
```python
list_1 = ['ABC', 'CAB', 'BDC', 'ABC']
```
위와 같은 list_1을 아래와 같이 key로 그 개수를 담고 싶다면

```python
dic = {'ABC': 2, 'CAB': 1, 'BDC': 1}
```

아래와 같이 counter 라이브러리를 사용하면 된다. Counter의 C는 대문자니까 작성시 주의한다.
```python
from collections import Counter

list_1 = ['ABC', 'CAB', 'BDC', 'ABC']
dic = Counter(list_1)
print(dic)
# 출력:
# Counter({'ABC': 2, 'CAB': 1, 'BDC': 1})
```