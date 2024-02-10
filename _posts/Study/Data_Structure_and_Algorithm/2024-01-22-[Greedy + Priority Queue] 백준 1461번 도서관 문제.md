---
title: '[Greedy + Priority Queue] 백준 1461번 도서관 문제'
author: baduk
date: 2024-01-22 22:55:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:<https://www.acmicpc.net/problem/1461>

## 문제 간단소개
0위치에 있는 책을 원래 자리로 되돌려야 하는데, 그때 걸어야하는 최소 걸음이 몇걸음인지 구하는 문제다. 한번 책을 옮길 때 들고갈 수 있는 책 권수가 정해져있고, 책을 되돌려 놓아야할 위치는 0보다 작거나 0보다 크다. 그리고 마지막에 책을 다 놓았다면 0위치로 돌아갈 필요없이 끝낼 수 있다.

## 문제풀이 아이디어 및 접근
책을 되돌려야할 위치가 음수일수도 양수일 수도 있다. 음수 위치에 있는 책을 돌려놓고 0위치를 그냥 지나 양수 위치에 있는 책을 되돌리고 0으로 되돌아가는 것은 최소걸음을 구해야하는 측면해서 비합리적이다. 따라서 이러한 상황을 제외시켜도 되기 때문에, 음수 위치 리스트와 양수 리스트를 따로 둔다.

그리고 한번에 들 수 있는 책의 권 수에 따라 묶어서 처리한다. 예를들어 한번에 들 수 있는 책이 두 권이면 두 권을 묶어서 처리하는 방식으로 걸음 수를 계산한다. 묶어줄 때 절대값이 큰 수의 왕복을 구하면 된다. 따라서 묶었을 때의 절대값이 큰 수의 곱하기 2를 하고 각각을 합한다.

이렇게 모든 묶음의 합을 구했다면, 마지막으로 모든 책을 반납하고 끝날 때는 0위치에 되돌아갈 필요가 없기 때문에, 이 묶음에대한 것만 편도로 계산해주어 모든 묶음의 왕복을 합한것에서 이 묶음음에 대한 편도를 빼준다. 이때 우리는 최소걸음을 구해야하므로 마지막 묶음은 여러 묶음중에 절대값이 가장 큰 값으로 계산한다. 가장 큰 값을 빼주어야 최소 걸음을 구할 수 있기 때문이다.

이렇게 계산한 값이 최종적인 구해야할 답인 최소 걸음이 된다.

## 제출코드
```python
n,m = map(int,input().split())
pos = list(map(int,input().split()))

list_m = list(filter(lambda x:x<0,pos))
list_m.sort()
list_p = list(filter(lambda x:x>0,pos))
list_p.sort(reverse=True)
sum_ = 0

for i in range(0,len(list_m),m):
    sum_ += abs(list_m[i])*2
for i in range(0,len(list_p),m):
    sum_ += list_p[i]*2

if len(list_m) > 0 and len(list_p) > 0:
    if abs(list_m[0]) > list_p[0]:
        sum_ -= abs(list_m[0])
    else:
        sum_ -= list_p[0]
else:
    if len(list_m) != 0:
        sum_ -= abs(list_m[0])
    else:
        sum_ -= list_p[0]
print(sum_)
```
내가 제출한 코드는 위와 같다. 코드 작성과정에서 계속 에러가 발생하고 원하는 값이 제대로 나오지 못해서 디버깅하는데 상당히 힘들었다. 머릿속에서 로직은 이미 알고있지만, 리스트를 여러 케이스에 맞게 구현해야 하다보니 인덱스 에러나 여러 상황들을 고려해야 해서 코드 작성이 조금 까다로웠다.

코드를 간단하게 설명하자면 처음에 음수 리스트와 양수리스트를 필터 람다로 list_m, list_p에 담았다.

그리고 각 리스트를 절대값이 큰 순서로 오도록 정렬하였고, 반복문안에서 체크할 인덱스를 들 수 있는 책의 권 수인 m에 맞게 반영하였다. 만약 들 수 있는 책의 권수가 2권이면 0, 2, 4, ...식으로 두 칸씩 띄엄띄엄 리스트에 접근한다.

그리고 아래 15번라인 조건문은 양수 또는 음수 리스트가 없어서 비어있을 수도 있기 걸어놓았다. 이 부분이 없으면 비어있는 리스트에서 인덱스에 접근하게되면서 인덱스에러가 발생하여 추가하였다. (사실 이러한 방식은 코드가 좀 지저분해지니까 이런식에 코드 작성은 지양해야 할 것이다.)

이런식으로 최종적으로 묶음중에 절대값이 큰 값을 한번 빼주는 연산으로 최소걸음수를 구하고 출력한다.

내가 작성한 코드는 알고리즘을 사용한 깔끔한 코드라고 보기 어려운 것 같다. 아래코드가 좀 더 개선된 코드다.

## 개선코드
```python
import heapq

n, m = map(int, input().split(' '))
array = list(map(int, input().split(' ')))
positive = []
negative = []

largest = max(max(array), - min(array))
# 절대값이 가장 큰 값을 구해놓는다. 이 값은 마지막 모든 묶음에 대한 왕복거리에서 
# 빼주어야하는 값이다. 최소걸음을 구하기위해는 큰값을 빼야하니까

for i in array:
    if i > 0:
        heapq.heappush(positive, -i) #힙큐는 기본적으로 최소힙이므로, 최대힙처럼 사용하기 위해 -를 붙여준다
    else:
        heapq.heappush(negative, i)

# 이런식으로 postive 리스트와 negative리스트로 부터 절대값이 큰 값을 우선적으로 pop시킬 수 있게된다.
result = 0

while positive:
    result += heapq.heappop(positive)
    for _ in range(m - 1):
        if positive:
            heapq.heappop(positive)
# postive리스트에 값이 다 없어질 때 까지 묶음에서 절대값이 큰 값을 result에 넣고
# 나머지는 pop시켜 그냥 버리도록한다.
# 반복의 텀은 들수있는 책의 권수인 m에 의해 결정될 것이다.
while negative:
    result += heapq.heappop(negative)
    for _ in range(m - 1):
        if negative:
            heapq.heappop(negative)
# negative 리스트도 위 positive리스트와 같은 방식으로 처리한다.
print(-result * 2 - largest)
# 맥스힙으로 사용하기 위해 리스트 안에는 모두 - 값이 들어가 있을 것이기 때문에
# -를 곱해주고 첫 부분에서 구했던 가장 큰 절대값을 빼주어 최종적인 값(최소걸음)을 구한다.
```
위 코드가 좀 더 알고리즘적으로 잘 풀어냈고, 코드도 깔끔하다. sort내장함수를 사용하지 않고 대신 힙을 사용하여 처리한다. 사실 시간복잡도상 큰 차이도 없어서 성능 상에서는 큰 차이는 없겠지만, 코드가 깔끔해서 보기에 이해하기 쉽고 디버깅하면서 헷갈림도 덜 해서 좋은 것 같다.