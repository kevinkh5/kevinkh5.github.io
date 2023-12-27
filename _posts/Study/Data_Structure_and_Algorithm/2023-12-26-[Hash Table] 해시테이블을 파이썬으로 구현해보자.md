---
title: '[Hash Table] 해시테이블을 파이썬으로 구현해보자'
author: baduk
date: 2023-12-26 12:59:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Hash Table, 해시테이블, python, problem solving, algorithm]
use_math: true
---
해시테이블은 특정 key를 가지고 value를 빠르게 찾을 수 있는 자료구조다.
해시테이블의 Constructor를 먼저 구현해보자

## Constructor 구현하기

```python
class HashTable:
    def __init__(self, size = 7):
        self.data_map = [None] * size
        
    def __hash(self, key):
        my_hash = 0
        for letter in key:
            my_hash = (my_hash + ord(letter) * 23) % len(self.data_map)
        return my_hash
    
    def print_table(self):
        for i, val in enumerate(self.data_map):
            print(i, ": ", val)
```

해시테이블은 각 키와 벨류를 저장해놓을 공간이 필요하므로 init부분에서 위 코드와 같이 `self.data_map = [None] * size`로 빈 리스트를 만들어주었다. 디폴트 사이즈를 7로 두었으므로 공간사이즈는 7일 것이다.

그리고 아래 __hash함수에서는 문자로된 키를 숫자로 변환시켜서 리턴해준다. 8번라인에서 ord()는 아스키문자를 숫자로 변환시켜서 리턴해주고, 그 옆에 23을 곱한 이유는 23이 소수이기 때문이다. 소수이기만 하면 23이 아닌 다를 수를 넣어도 된다. 그리고 %(모듈러)를 사용하면 특정범위의 수만 나오게 되는데, 예를들면 data_map의 사이즈가 7이라면 %(모듈러)를 사용했을 때 0~6사이에 숫자만 나올 것이다.

그 아래 print_table은 테이블에 들어간 key와 value가 무엇인지 출력해준다.

## Set 구현하기
해시테이블의 set메소드는 테이블안에 key와 value를 삽입하도록 도와주는 메소드다.
```python
    def set_item(self, key, value):
        index = self.__hash(key)
        if self.data_map[index] == None:
            self.data_map[index] = []
        self.data_map[index].append([key, value])
```
위 코드를 보면 알 수 있듯이, key에 문자를 입력받으면 해시메소드를 통해서 특정 수가 index 변수로 반환되고 이 index 위치에 value를 넣는 것이다. 이 때 해당 index가 None으로 비어있으면 [ ]빈 리스트를 넣어서, 리스트안에 key와 value가 들어갈 공간을 만들어준다.

## Get 구현하기
해시테이블의 get메소드는 해시테이블에 저장된 특정 key에 대한 value를 반환받기 위해서 사용한다. 하지만 Get메소드에서는 저장되지 않은 key를 찾으려 하면 None값이 반환될 것이다. 코드는 아래와 같다.
```python
    def get_item(self, key):
        index = self.__hash(key)
        if self.data_map[index] is not None:
            for i in range(len(self.data_map[index])):
                if self.data_map[index][i][0] == key:
                    return self.data_map[index][i][1]
        return None
```
위 코드를 보면 먼저 key를 받아서 index를 알아낸 후 그 index위치로 간다. 만약 그 index위치가 비어있으면 None을 반환한다. 만약 있다면 그 index위치에서 key값이 같은 것이 있는지 for문 안에서 하나씩 찾아보면서 일치하는 것이 있으면 그 해당 index안에 있는 해당 key의 value를 반환한다. 만약 없다면 for문을 빠져나와서 None을 반환하게 될 것이다.

위 코드는 for-in으로 간단하게 아래 처럼 쓸 수도 있다.

```python
    def get_item2(self, key):
        index = self.__hash(key)
        if self.data_map[index] is not None:
            for _ in self.data_map[index]:
                if _[0] == key:
                    return _[1]
        return None
```

## Keys 구현하기
keys 메소드는 해시테이블이 가지고 있는 모든 키를 리스트안에다가 넣어서 출력해주는 메소드다. 코드는 아래와 같다.
```python
    def keys(self):
        all_keys = []
        for i in range(len(self.data_map)):
            if self.data_map[i] is not None:
                for j in range(len(self.data_map[i])):
                    all_keys.append(self.data_map[i][j][0])
        return all_keys
```
먼저 all_keys라는 빈 리스트를 만들어준다. 이 곳에 해시테이블에 저장된 key를 담을 것이다. 그리고 테이블의 인덱스를 쭉 돌면서 None이 아닌 것을 찾고 그 곳에서 다시 리스트 안에 있는 key를 하나씩 all_keys에 담는다. 이 모든 과정이 끝나면 all_keys를 반환한다.

## 해시테이블의 Big O

해시테이블은 리스트안에 한 공간에만 key와 value가 특별히 막 쌓여있지않고 잘 분배되어있으면, 시간복잡도가 O(1)이다. 하지만 한쪽 공간에만 만약 쌓여있는 최악의 경우 O(n)이 될 수도 있다.