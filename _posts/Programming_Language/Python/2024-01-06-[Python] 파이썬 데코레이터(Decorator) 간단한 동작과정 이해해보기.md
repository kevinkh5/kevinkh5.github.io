---
title: '[Python] 파이썬 데코레이터(Decorator) 간단한 동작과정 이해해보기'
author: baduk
date: 2024-01-06 15:18:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬, 데코레이터, decorator, 파이썬 문법]
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 데코레이터(Decorator) 코드 예시
```python

import time

def delay_decorator(function):
    def wrapper_function():
        time.sleep(2)
        function()
    return wrapper_function

@delay_decorator
def say_hello():
    print("Hello")

say_hello()
```
데코레이터(@)가 붙은 함수는 처음에 run을 하면 자동으로 실행이 되는 듯 하다. 따라서 위 코드는 처음 프로그램 실행시 @가 붙은 함수인 delay_decorator가 자동으로 실행이 될 것이다. 만약 위 코드에 작성된 delay_decorator 함수내에서 return wrapper_function에 괄호 -> ()를 추가하여 return wrapper_function`()`으로 작성하게되면 say_hello함수를 say_hello()로 실행하지 않더라도 delay_decorator함수내에 있는 wrapper_function 함수가 자동으로 불려지는 것 같다.

따라서 say_hello()로 실행했을 때만 wrapper_function이 불려질 수 있도록 하려면 반드시 return wrapper_function()이 아니라 return wrapper_function으로 작성해야겠다. 

## 데코레이터 코드 예시2
```python
def delay_decorator(function):
    def wrapper_function():
        time.sleep(2)
        function()
    return wrapper_function

@delay_decorator
def say_hello():
    print("Hello")

a = delay_decorator(say_hello)
a()
```
맨 위 첫번째 예시코드처럼 데코레이터가 달린 함수(say_hello)를 불러서 데코레이터 함수를 실행시키는 방법 외에도, 데코레이터 자체를 가지고 데코레이터 함수를 실행하는 방법도 있다. 위 코드와 같이 데코레이터함수 안에 실행시킬 함수를 직접 넣어서 그 주소를 반환받아 실행시킬 수도 있다.

## 데코레이터 간단 활용 예시(속도 계산기)
```python
import time

def speed_calc_decorator(function):
    def wrapper_function():
        start_time = time.time()
        function()
        end_time = time.time()
        print(function.__name__)
        print(end_time-start_time)
    return wrapper_function

@speed_calc_decorator
def fast_function():
    for i in range(1000000):
        i * i

@speed_calc_decorator
def slow_function():
    for i in range(10000000):
        i * i

fast_function()
slow_function()
```
데코레이터를 활용하면 두 함수의 속도를 측정하고 비교하는데에서도 써먹을 수 있을 것 같다. 위 코드에 대한 결과는 아래와 같다.

```text
fast_function
0.050369977951049805
slow_function
0.493243932723999
```

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>
