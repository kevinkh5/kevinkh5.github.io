---
title: '[Merge Sort] 합병정렬을 파이썬으로 구현해보자'
author: baduk
date: 2024-01-03 11:17:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Merge Sort, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 합병정렬(Merge Sort)
합병정렬은 나열된 수 들을 반씩 쪼개어 나가면서 1개만 남을 때까지 나누고, 그것을 각각을 다시 병합하는 과정에서 정렬 해나가는 정렬 알고리즘이다.

## 합병 메소드 구현하기
먼저 합병 정렬 하는 과정에서, 두 리스트를 받아서 두 리스트를 합치면서 정렬하는 메소드를 구현해보겠다.
```python
def merge(list1, list2):
    combined = []
    i = 0
    j = 0
    while i < len(list1) and j < len(list2):
        if list1[i] < list2[j]:
            combined.append(list1[i])
            i += 1
        else:
            combined.append(list2[j])
            j += 1
    while i < len(list1):
        combined.append(list1[i])
        i += 1
    while j < len(list2):
        combined.append(list2[j])
        j += 1
    return combined
```
위 코드의 로직은 두 리스트의 각 원소를 하나씩 비교하면서, 숫자가 더 작은 것을 먼저 combined리스트에 append를 시켜나가면서 나열을 진행한다.


## 합병정렬 구현하기
이제 위에서 작성했던 합병 메소드를 합병 정렬 함수에서 사용하면 된다. 합병정렬 함수 안에서는 리스트안에 수가 1개가 들어갈 때 까지, 재귀적으로 불려지면서 반으로 계속 쪼개진다. 그리고 리스트안에 수가 1개가 되면 리턴되면서 left와 right에 값이 반환이 되고 다시 그 값을 merge메소드에 넣어주면 정렬된 수가 들어간 리스트를 반환 받는다. 그리고 그 리스트를 다시 left 또는 right로 반환받고(처음에는 left, 두번째는 right가 될 것이다.) merge메소드에 다시 입력하면 정렬된 값을 받게 될 것이다. 이러한 방식으로 최종적으로는 정렬된 리스트를 반환받게 될 것이다.
```python
def merge_sort(my_list):
    if len(my_list) == 1:
        return my_list
    mid = int(len(my_list)/2)
    left = merge_sort(my_list[:mid])
    right = merge_sort(my_list[mid:])
    return merge(left,right)
```

## 합병정렬의 Big-O
합병정렬의 시간복잡도는 $O(N*logN)$이다.
일단 위 merge메소드 코드를 보면 알 수 있듯, merge메소드의 시간복잡도가 n이고, 아래 merge_sort함수에서는 시간복잡도가 log(n)이 나오기 때문이다. 아래 그림을 보면 알 수 있듯, 숫자의 개수가 4개일 경우 2번의 분할과정을 거쳤다가 다시 합쳐진다. 즉 다른 말로하면 개수가 4개(=$2^2개$) 일 때, 2번의 일을 한다. 개수가 8개(=$2^3개$)일 때는 3번의 일을 한다. 즉 개수가 $2^N$개 있으면 n번 일하므로, 개수가 N개 일때는 $log(n)$번 일한다. 

<img src = "https://lh3.googleusercontent.com/pw/ABLVV87bqaBwXKdbvgP3IpQdfw0Xufm0j8rmAYMQjxrkiv1UCz0lTKdZjeR17t4Lga5jjAjWhSlPVcY9E-QouWEaGdd7-H7U_8ZbpT7S5QNke3tNUEiE3wCDKFgXc2FPJp2FCccY2VFXs35jP02psF01qLU3=w2720-h942-s-no-gm?authuser=0" alt="exam" width="400" height="400">


따라서 merge_sort안에서 merge메소드를 제외한 시간복잡도는 $log(n)$이고 merge메소드와 함께 고려하면 결국 합병정렬의 시간복잡도는 O(n*log(n))이 된다. 합병정렬은 시간복잡도가 $O(n^2)$을 보이는 버블정렬, 선택정렬, 삽입정렬과 비교했을때 훨씬 효율적이다.

### 재귀적으로 진행돼서 시간복잡도 구하기 복잡할 수 있지만..
합병정렬의 시간복잡도를 구할 때 조금 어려울 수 있는게, 합병정렬이 재귀적인 방법(콜스택)으로 진행돼서 계산하기가 어려울 수 있다. 하지만 이렇게 생각하면 간단할 것이다. merge_sort가 1회가 완전히 끝나려면 left,right를 모두구하고 return merge(left,right)까지 끝나야 한다.

처음에 merge_sort를 다시 호출하여 left에 값을 넣어주는데, 그 때 left부분에서 merge_sort를 몇 번 호출했는지만 고려하면된다. left 부분에서 merge_sort를 한번 호출 했다면 right부분도 결국 한번 호출 된 것이고, merge_sort가 재귀적으로 불러지는 것은 한번이다.(정확히는 처음 불렀을 때 것 까지 고려하면 한번을 더해야 할 것이다. 그래서 정확히는 두번이다.) left부분에서 5번 불리고 right부분에서 5번 불렸다고 해서 10번 불러진게 아니다. left부분 5번 불렸으면 그냥 5번 불려진 것이다. 

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>