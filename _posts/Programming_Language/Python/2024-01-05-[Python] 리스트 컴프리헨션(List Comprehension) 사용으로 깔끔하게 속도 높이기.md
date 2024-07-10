---
title: '[Python] 리스트 컴프리헨션(List Comprehension) 사용으로 깔끔하게 속도 높이기'
author: baduk
date: 2024-01-05 12:08:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬]
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

코딩테스트 문제를 풀다가 파이썬 리스트 컴프리헨션을 사용해서 코드를 아래와 같이 작성해봤다.
```python
def solution(array, commands):
    return [sorted(array[_[0]-1:_[1]])[_[2]-1] for _ in commands]
```

나는 개인적으로 가독성이 떨어지는 코드가 싫어서, 위와 같이 읽기 다소 어려운 코드는 잘 작성하지 않는다. 뭔가 압축적이면서 간결한듯 보이나, 뭔가 계속보고 있으면 눈이아프다.

위 코드를 펼쳐보면 사실 아래코드와 같이 작성할 수 있다. 아래코드가 읽기에는 훨씬 더 수월하다.
```python
def solution(array, commands):
    results = []
    for _ in commands:
        results.append(sorted(array[_[0]-1:_[1]])[_[2]-1])
    return results
```
하지만 리스트 컴프리헨션을 사용했을 때, 속도가 훨씬 더 빠르다고 한다.
앞으로는 퍼포먼스를 높이기 위한 코드를 작성할 때 가능한 리스트 컴프리헨션을  사용하도록 해야겠다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>