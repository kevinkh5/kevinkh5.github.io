---
title: '[Quick Sort] 퀵정렬을 파이썬으로 구현해보자'
author: baduk
date: 2024-01-04 07:00:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Quick Sort, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 퀵정렬(Quick Sort)
퀵정렬도 합병정렬과 마찬가지로 devide & conquer 방법으로 정렬하는 알고리즘이다. 퀵정렬은 합병정렬과 다르게 Pivot이라는 것을 둔다. 그리고 그 Pivot을 기준으로 작은 숫자를 왼쪽으로 보내서, 결과적으로는 Pivot의 왼쪽 수의 나열은 Pivot보다 작은수가 되고, Pivot의 오른쪽 수의 나열은 Pivot보다 큰 수가 오게 된다. 따라서 Pivot의 위치는 최종적으로 정렬이 마무리 되었을 때의 위치와 같아진다. 즉 Pivot의 자리는 이제 정렬이 되었으므로 그 위치를 건드릴 필요가 없어진다.

Pivot이 정렬되었으면, 그 다음부터는 Pivot의 왼쪽 부분과 오른쪽 부분에 대해서 정렬을 진행해야 할 것이다. 먼저 퀵정렬에서 중요한 기능을 하는 pivot을 아래에서 구현해보겠다.

## Pivot 구현하기
```python
def swap(my_list, index1, index2):
    temp = my_list[index1]
    my_list[index1] = my_list[index2]
    my_list[index2] = temp
    

def pivot(my_list, pivot_index, end_index):
    swap_index = pivot_index
    
    for i in range(pivot_index+1, end_index+1):
        if my_list[pivot_index] > my_list[i]:
            swap_index += 1
            swap(my_list, swap_index, i)
    swap(my_list, swap_index, pivot_index)
    return swap_index
```
먼저 pivot_index와 swap_index를 맨 왼쪽 인덱스에 둔다. for문이 진행되면서 i는 맨 왼쪽 두번째 인덱스 부터 시작하며, i는 한 루프당 오른쪽으로 한칸 씩 이동하게 될 것이다. i와 pivot_index가 가리키는 값을 서로 비교해서 i가 가리키는 값이 pivot_index가 가리키는 값보다 작다면 swap_index를 오른쪽으로 한칸 옮겨 주고, swap_index가 가리키는 값과 i가 가리키는 값을 바꿔준다.

이런식으로 i가 인덱스 전체를 다 지나고나서 swap_index가 가리키는 값과 pivot_index가 가리키는 값을 바꿔주는데, 그 이유는 이렇게 바꿔주게되면 결과적으로 pivot_index가 가리켰던 값보다 작은 값이 전부 swap_index의 왼쪽편으로 가게 될 것이고, swap_index에 위치한 값은 정렬될 수 있기 때문이다.

이제 swap_index의 위치한 값은 정렬되어 더 이상 바꿔줄 필요가 없기 때문에, swap_index를 리턴하고, 앞으로 이 swap_index의 왼쪽편과 오른쪽편에 있는 수들을 정렬하도록 하여 전체적으로 정렬해나갈 수 있도록 하면 될 것이다.


## Quick Sort 구현하기
이제 위에서 구현한 Pivot을 아래 Quick Sort함수에서 사용하면된다.

```python
def quick_sort_helper(my_list, left, right):
    if left < right:
        pivot_index = pivot(my_list, left, right)
        quick_sort_helper(my_list, left, pivot_index-1)
        quick_sort_helper(my_list, pivot_index+1, right)
    return my_list

def quick_sort(my_list):
    return quick_sort_helper(my_list,0,len(my_list)-1)

```
quick_sort_helper 메소드를 보면, 처음에 left에는 리스트의 맨 왼쪽 인덱스가 들어가고, right에는 맨 오른쪽 인덱스가 들어간다. 그리고 pivot함수를 거쳐서 pivot_index를 구하고 pivot_index를 기준으로 양쪽에있는 수의 나열에 대해서 다시 재귀적으로 quick_sort_helper함수를 부른다. 이런식으로 계속해서 정렬해나간다.

if left < right 조건이 들어간 이유는 무한 재귀를 피하기 위해서이다. 재귀적으로 부르는 과정에서 어느 순간 pivot_index가 1이 나오게 되는데, 이때 인덱스가 1인 수의 왼쪽에 있는 수는 인덱스가0인 숫자 하나 밖에 없으므로 정렬할 필요가 없어진다. 따라서 이러한 상황에 대해서는 pivot함수를 부르거나 재귀를 반복하는것을 방지하기 위해 넣은 것이다. 만약 이 조건문을 넣지 않으면 quick_sort_helper함수를 무한 재귀 하는 일이 발생할 것이다.


## 퀵 정렬의 Big O
퀵 정렬은 굉장히 효율적인 시간복잡도를 가진 알고리즘이다. 베스트 케이스와 평균 케이스 모두 $O(n*log(n))$ 이다. 그 이유는 pivot함수에서 일어나는 시간복잡도를 제외하고, pivot을 기준으로 나누어서 재귀하는 과정만 따졌을때의 시간복잡도가 $O(log(n))$이고, pivot함수에서 한번의 정렬이 일어나는 과정의 시간복잡도가 $O(n)$이기 때문이다.

하지만 퀵 정렬의 최악의 케이스의 시간복잡도는 $O(n^2)$인데, 그 이유는 이미 정렬된 리스트를 퀵 정렬하게 될 때의 시간복잡도가 $O(n^2)$이기 때문이다. 따라서 이미 거의 다 정렬된 리스트를 정렬할 때는 퀵 정렬보다는 삽입정렬을 사용하는 것이 훨씬 효율적이다. 삽입정렬은 거의 다 정렬된 리스트에 대해서 시간복잡도가 거의 $O(n)$이기 때문이다.

그렇다면 왜 퀵 정렬은 이미 정렬된 리스트에서 시간복잡도가 $O(n^2)$일까?
그 이유는 이미 정렬된 리스트에 대해 pivot함수를 실행하면 left에 넣었던 인덱스 값(처음에 그 값은 0일 것이다)을 그대로 pivot_index로 반환받고, 그 pivot_index를 1씩 증가 시켜가면서 재귀를 진행하기 때문에, 결국 재귀가 n번 일어난다. 즉 pivot함수를 n번 실행하게 되는 꼴이고, pivot함수 한번 일어나는데 시간복잡도가 $O(n)$이니까 $n*n$해서 결국 시간복잡도는 $O(n^2)$이 된다.


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>