---
title: '[Basic Sorts] 버블정렬, 선택정렬, 삽입정렬을 파이썬으로 구현해보자'
author: baduk
date: 2024-01-02 05:29:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Bubble Sort, Selection Sort, Insertion Sort, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 버블정렬(Bubble Sort)
버블정렬은 가장 큰 수를 맨 오른쪽으로 보내는 식으로 계속해서 정렬해나간다. 앞에서 부터 두 수를 비교하고 큰 수가 오른쪽으로 오도록 스위치하는 식으로 진행된다. 코드는 아래와 같다.

```python
def bubble_sort(my_list):
    for i in range(len(my_list)-1,0,-1):
        for j in range(0,i):
            if my_list[j] > my_list[j+1]:
                my_list[j], my_list[j+1] = my_list[j+1], my_list[j]
    return my_list
```

## 선택정렬(Selection Sort)
선택정렬은 왼쪽부터 수의 크기비교를 시작하는데, 최소값인 인덱스를 따로 찾아서 저장하고, 최소값인 인덱스를 맨 왼쪽으로 차례도록 보내도록 스위치하면서 정렬한다. 왼쪽에 정렬된 인덱스 다음 것부터 계속 최소값을 찾아서 또 자리를 스위치한다. 코드는 아래와 같다.

```python
def selection_sort(my_list):
    for i in range(len(my_list)-1):
        min_index = i
        for j in range(i+1,len(my_list)):
            if my_list[min_index] > my_list[j]:
                min_index = j
        if i != min_index:
            my_list[i], my_list[min_index] = my_list[min_index], my_list[i]
    return my_list
```

## 삽입정렬(Insertion Sort)
삽입정렬은 인덱스 1번부터 쭉 이동하면서 왼쪽 값과 비교하고, 만약 비교했을때 왼쪽 값이 더 크면 자리를 스위치한다. 왼쪽 값이 더 작다면 다음 인덱스로 넘어간다. 만약 스위치하고나서도 새로운 왼쪽 값과 비교했을 때, 새로운 왼쪽 값이 더 크면 또 스위치 한다. 이와 같은 과정은 1번 인덱스부터 마지막 인덱스까지 반복한다.

삽입정렬은 두 가지 코드로 작성해보았다. 먼저 첫번째는 아래와 같이 두개의 for문으로 작성한 코드다.
### 삽입정렬 코드1
```python
def insertion_sort(my_list):
    for i in range(1,len(my_list)):
        for j in range(i,0,-1):
            if my_list[j-1] > my_list[j]:
                my_list[j-1], my_list[j] = my_list[j], my_list[j-1]
            else:
                break
    
        print(my_list)
    return my_list
```
위 코드는 왼쪽값과 비교했을 때, 왼쪽 값이 더 클 경우 스위치하고 그렇지 않으면, break로 반복문을 나오는 방식이다.

### 삽입정렬 코드2
```python
def insertion_sort(my_list):
    for i in range(1,len(my_list)):
        temp = my_list[i]
        j = i-1
        while temp < my_list[j] and j > -1:
            my_list[j+1] = my_list[j]
            my_list[j] = temp
            j -= 1
    return my_list
```
위 코드는 temp에 값을 미리 저장하고 왼쪽값이 더 클 경우 해당 위치에, 왼쪽값을 오른쪽으로 이동시키고, temp에 있는 값을 왼쪽 값에 전달해나간다. 만약 왼쪽 값이 더 작은 경우, 스위치할 필요가 없으므로 while문 반복을 시도하지 않고 다음 차례로 넘어간다. 또한 j가 -1보다 크게 될 경우에도 다음 차례로 넘어간다.


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>