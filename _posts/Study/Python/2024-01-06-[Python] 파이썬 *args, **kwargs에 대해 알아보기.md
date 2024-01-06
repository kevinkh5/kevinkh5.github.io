---
title: '[Python] 파이썬 *args, **kwargs에 대해 알아보기'
author: baduk
date: 2024-01-06 19:18:00 +0900
categories: [Study, Python]
tags: [Python, 파이썬, 파이썬 문법]
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## args 란?
args는 arguments의 약자이고 많은 양의 argument를 함수로 받을 때 사용한다. 아래는 사용 예시 코드다.

```python
def example_function(*args):
    for arg in args:
        print(arg)

example_function(1, 2, 3, 'hello')
# Output: 1, 2, 3, hello
```

## kwargs 란?
kwargs는 keyword arguments 의 약자로 키워드를 개수 상관없이 받아서 딕셔너리로 받을 때 사용한다. 아래는 코드 예시다.

```python
def example_function(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

example_function(name='Alice', age=30, country='USA')
# Output: name: Alice, age: 30, country: USA
```
위 코드에서 함수안에서 kwargs는 딕셔너리 형태의 {'name': 'Alice', 'age': 30, 'country': 'USA'}을 받는다.


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>
