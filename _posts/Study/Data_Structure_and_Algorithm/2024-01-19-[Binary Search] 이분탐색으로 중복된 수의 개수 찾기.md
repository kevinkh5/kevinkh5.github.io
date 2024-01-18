---
title: '[Binary Search] 이분탐색으로 중복된 수의 개수 찾기'
author: baduk
date: 2024-01-19 08:18:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
## 이분탐색으로 중복된 수의 개수를 찾는 방법
이분탐색으로 중복된 수의 개수를 찾기 위해서는, 두 가지 인덱스를 알아야 한다. 첫번째는 찾고자 하는 수가 처음으로 나오는 인덱스고, 두번째는 찾고자 하는 수 보다 처음으로 커지는 인덱스다.

찾고자 하는 수가 처음으로 나오는 인덱스의 위치를 다른 말로 Lower Bound(하한 경계)라고 하고, 찾고자 하는 수 보다 처음으로 커지는 인덱스의 위치를 Upper Bound(상한경계)라고 한다.

<img src="https://lh3.googleusercontent.com/pw/ABLVV86SVZ6OT8G8wXE0UqLAo7ZH_0-m7zpkYdPTNy8owgJ4T0wkK5O7n7VSoI_5-43YJsyEp1zg4HQBa5ywRNWOVD3Q65OVuLmcn5Ucd_q2Uoz_i1o_SZmcuB75jbBleD-mzpuRczeBNT1RlsCIxTP5Xy7Q=w1278-h1154-s-no-gm?authuser=0" alt=exam width=400>

Upper Bound에 위치한 인덱스에서 Lower Bound에 위치한 인덱스를 빼주면, 찾고자 하는 수의 개수를 구할 수 있다.

## Lower Bound & Upper Bound 파이썬으로 구현해보기
```python
def binary_search_lower_bound(arr, target):
    start = 0
    end = len(arr) - 1
    while start <= end:
        mid = (start + end) // 2
        if arr[mid] >= target:
            end = mid - 1
        else:
            start = mid + 1
    return start

def binary_search_upper_bound(arr, target):
    start = 0
    end = len(arr) - 1
    while start <= end:
        mid = (start + end) // 2
        if arr[mid] > target:
            end = mid - 1
        else:
            start = mid + 1
    return start

arr = [1,2,2,2,2,3]

print(binary_search_lower_bound(arr, 2)) # 1
print(binary_search_upper_bound(arr, 2)) # 5
```
lower bound와 upper bound의 파이썬 코드는 위와 같다. lower bound와 upper bound함수 둘 다 공통 적으로는 리스트의 mid지점이 내가 찾고자 하는 수 보다 크면, 그 보다 작은 범위를 탐색해야 하므로 end는 mid - 1 로 옮겨준다.

하지만 두 함수의 차이점은 lower bound함수의 경우 리스트의 mid지점이 찾고자 하는 수와 같으면 작은 범위탐색을 유도하도록 end를 mid-1로 옮겨주지만, upper bound함수의 경우 리스트의 mid지점이 찾고 하는 수와 같으면 오른쪽 범위 탐색을 유도하도록 start를 mid+1 옮겨준다.

요약하자면 타겟넘버보다 더 큰 수나 더 작은 수를 만나면 옮겨주는 방식은 같지만, 타겟 넘버와 같은 수를 만나면 start를 오른쪽으로 옮길지 end를 왼쪽으로 옮길지는 다르다.


## 중복된 수 개수 찾기
```python
target_count = binary_search_upper_bound(arr, 2) - binary_search_lower_bound(arr, 2) # 4
```