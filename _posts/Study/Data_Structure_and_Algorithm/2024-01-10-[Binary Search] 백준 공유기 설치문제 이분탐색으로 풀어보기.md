---
title: '[Binary Search] 백준 공유기 설치문제 풀어보기(이분탐색)'
author: baduk
date: 2024-01-10 21:31:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

출처:<https://www.acmicpc.net/problem/2110>

## 문제소개 (가장 인접한 공유기 설치집 사이의 최대거리 구하기)
일단, 이 문제는 공유기 설치 가능한 개수를 구하는 문제가 아니라, 공유기 개수는 이미 정해놓고, 그 개수를 만족하는 여러가지 경우 중 공유기가 설치된집 사이에서의 가장 인접한(가까운)  거리가 최대로 나오는 경우에서의 그 거리를 구하는 것이다.

(사실 난 처음에 이 문제를 이해하는 것도 쉽지않았다...)

## 문제해결 아이디어
설치된 공유기 개수가 3으로 정해져있다고 해보자. 그리고 만약 가장 인접한거리를 4이상이라고하면,즉 최소 4이상의 거리를 띄워야지만 공유기를 설치할 수 있다고 해보자. 만약 4이상의 간격을 띄워서 공유기 개수를 채우는데, 공유기 개수가 3개 이상으로 설치될 수 있으면 간격을 조금 더 넓혀 볼 것을 고려해 볼 수 있을 것이다. 일단 4이상의 간격을 띄워서 공유기 3개 이상 설치가 가능하다는 소리는 간격을 4미만으로 잡았을때 무조건 3개이상 설치가 가능하다는 소리다. (간격을 좁게하면 공유기를 따닥따닥 붙여서 더 많이 설치할 가능성을 높일 수 있으니까)
따라서 공유기 3개 이상 설치가 현 시점에서 가능하다고 했을때 지금 까지 구한 가장 인접한 간격의 최대거리는 4가 될 것이다. 하지만 아직 최대거리를 확신할 수는 없다. 가장 인접한 간격을 5이상으로는 안해봤으니까. 앞으로는 간격을 5이상으로 조금 더 늘려서 그 최대거리를 찾아가봐야 할 것이다.

하지만 간격을 5, 6, 7 키워가다보면 어느순간 공유기 개수가 3개에 못미치고 2개까지만 설치되는 시점이 올 수 있다. 그러면 그때는 간격을 다시 줄이는 방향으로 가서 공유기 개수 3개를 만족시킬 수 있도록 해야할 것이다.

 이 문제에서 구하고자 하는 것은 최대거리, 즉 최대간격을 구하는 것으로 가능하다면 간격을 넓혀가야한다. 만약 공유기 설치 개수를 만족 못하면 간격을 줄여야한다. 따라서 이 간격을 늘어나는방향 또는 줄어드는방향으로 왔다갔다하면서 찾아가는 방식을 이분탐색으로 진행하면 된다.


## 생각해야 할 포인트
인접한거리를 잡고, 공유기 개수를 채울때, 가능한 많은 공유기가 설치될 수 있도록 첫번째 집에 공유기를 설치하고 들어가야한다. 가능한 많은 공유기가 설치될 수 있도록 하는 이유는 가장인접한 최대거리를 구하기 위한 방향이기 때문이다. 예를 들어서 공유기 설치 개수가 3이라고 정해져있을때,  특정한 인접한 거리조건에서 공유기 설치개수가 최대로 채웠을 때 4가 나오면 거리조건을 높여볼만 해지고, 따라서 가장인접한 최대거리를 더 키울 수 있는 여지가 생기게된다. 즉 최대거리를 구해야하기때문에, 공유기 개수를 최대한 채우려고 하는 것이다.


## 풀이

#1.집 좌표를 보고 최대간격과 최소간격을 구한다.(물론 이전에 미리 정렬을 해놔야 함)

#2.그 중간값을 가장 인접한 간격이라고 했을때 공유기 개수가 만족되는지 체크한다.

#3-1.공유기 개수가 충분히 만족되면, 그 인접한 간격 값을 저장해놓고, 그 인접한 간격을 min으로 잡고 다시 그 중간값으로 2번과정 반복(인접한 간격이 커지는 방향으로 체크)

#3-2.공유기 개수를 만족시킬 수 없다면 그 인접한 간격을 max로 잡고 다시 중간값으로 2번과정 반복(인접한 간격이 작아지는 방향으로 체크)

#4.min과 max가 같아지는 시점을 지나서 min > max 가 되면 저장된 간격 값이 답이됨.

## 후기
사실 이진탐색의 개념은 그렇게 어렵지 않은데, 이 문제를 푸는 핵심 아이디어를 떠올리는 것 자체가 어려워서 문제가 어렵다. 이진탐색이 어려운것 보다 이 문제속에서 공유기 설치를 특정 간격을 기준잡아 공유기 설치 가능 개수를 파악 한다는 그 핵심적인 아이디어을 떠올리는게 쉽지않다.

최소간격과 최대간격의 중간으로 간격을 찾아나가는데, 그 최소 간격으로 설치할 수 있는 공유기 개수를 구하고, 설치가 충분히 가능하면 간격을 늘리고, 설치하기에 충분치 않다면 간격을 줄여 나간다는 아이디어, 그리고 공유기 개수를 최대한 넣기위해 첫번째 집부터 공유기 설치하고 시작하는 아이디어 등등.. 쉽지 않다.

이 아이디어가 이 문제의 80퍼센트고 20퍼센트가 이진탐색이 아닐까 싶다. 이분탐색을 모르면 무조건 시간복잡도 문제에 걸려서 못 풀겠지만, 단순히 이분탐색을 안다고 풀 수 있는 문제도 아닌것 같다.

## 내가 작성한 코드
```python
N, C = map(int,input().split())
arr = [int(input()) for i in range(N)]
arr.sort()

min_ = arr[1] - arr[0]
max_ = arr[-1] - arr[0]
for i in range(2,len(arr)):
    if min_ > arr[i] - arr[i-1]:
        min_ = arr[i] - arr[i-1]

mid = (min_ + max_) // 2
result = 0
while min_ <= max_:
    p = arr[0]
    count = 1
    for i in range(1, len((arr))):
        if arr[i] - p >= mid:
            p = arr[i]
            count += 1
    if count < C:
        max_ = mid - 1
    else:
        result = mid
        min_ = mid + 1
    mid = (min_ + max_) // 2
print(result)
```


<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>