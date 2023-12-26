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