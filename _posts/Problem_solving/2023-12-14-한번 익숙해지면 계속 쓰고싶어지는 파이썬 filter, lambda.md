---
title: '한번 익숙해지면 계속 쓰고싶어지는 파이썬 filter, lambda'
author: baduk
date: 2023-12-14 15:45:00 +0900
categories: [Problem solving, 바둑이의 코딩일기🧑‍💻]
tags: [코딩일기, Problem Solving]
---

## 풀이과정
풀었던 문제는 LV1 문제다. 코드를 구현하는것은 어렵지 않았는데 처음에 문제를 이해하는데 시간이 조금 걸렸다. 뭔가 데이터를 제외시키고, 리스트의 특정 위치에 대한 데이터를 기준으로 정렬하고 하는게 괜히 복잡하게 느껴졌다. 하지만 문제를 이해하고 나서 코드를 구현하기까지는 금방 끝났다. 정렬을 해야하는 부분은 당연히 sort라이브러리를 써야겠다는 생각을 했고, 데이터를 제외시키는것은 list/filter/lambda를 쓰면 좋겠다는 생각이 들었다. 나는 가능하면 for문을 쓰지않고 lambda를 사용하면 퍼포먼스가 좋고 코드가 간결할 것 같아서 lambda로 해결했다. 사실 for문 보다도 lambda쓰는게 더 간단한 듯 하다. 코드는 간단하게 아래와 같이 작성하였다.

## 내가 작성한 코드

```python
def solution(data, ext, val_ext, sort_by):
    dict_index = {'code':0,'date':1, 'maximum':2, 'remain':3} # 특정 데이터에대한 위치를 딕셔너리를 통해 접근하기 위해 만든것
    list_filtered = list(filter(lambda x: x[dict_index[ext]]<val_ext, data)) # 조건에 맞추어 제외시키기
    list_filtered.sort(key = lambda x : (x[dict_index[sort_by]])) # 남은 데이터를 특정 데이터에 대해서 어센딩으로 정렬하기
    return list_filtered
```