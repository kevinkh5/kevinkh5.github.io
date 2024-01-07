---
title: '[Dynamic Programming] 동적계획법을 냅색문제(Knapsack Problem)에 적용해서 풀어보자'
author: baduk
date: 2024-01-07 18:34:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Dynamic Programming, DP, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 배낭문제(냅색문제)
배낭문제는 배낭에 물건을 집어넣는 여러가지 경우의 수 중에서 가장 큰 가치가 들어갈 수 있도록 하는 문제다. 물건을 집어넣을 때, 무게제한이라는 조건을 고려해야한다. 물건을 집어넣는 모든 경우의 수를 따져서 풀려고하면 시간복잡도가 $O(2^n)$이 되기때문에, 무작정 모든 경우의수를 구해서 풀면 안된다.

## 풀이
아이디어는 물건을 1개씩 넣어가면서, 무게 제한에 따라서 최대가치를 각각 구해나가는 식으로 접근해야한다. 물건을 새로 넣어주기 전과 새로 넣은 후의 변화된 가치를 비교하고 더 큰 가치로 업데이트 시켜나간다. 새로들어올 물건의 가치와 이전에 구한 것(계산한 것)을 함께 이용해야 한다. 따라서 이 문제는 다이나믹 프로그래밍을 이용하여 해결할 수 있다.

물건을 집어 넣고 무게제한에 따라서 가치를 구할 때, 만약 무게제한이 새로들어온 물건의 무게보다 작다면, 새로들어온 물건의 가치가 어떻든간에 영향을 주지 않을 것이다. 즉, 이때는 이전에 구한 가치를 그대로 업데이트 시키면 된다. `하지만 무게제한이 새로들어온 무게와 같거나 커지는 순간, 새로들어온 물건의 영향이 생길 수 있다. 만약 (새로들어온 물건의 가치) + 새로 들어온 물건이 없었을때, 즉 이전에 (무게제한 - 새로들어온 물건의 무게 = 새로들어온 물건의 무게를 뺀 나머지)무게에 대한 최대가치가 더 크면 새롭게 가치가 업데이트 될 것이다.`

방금 설명한 이 부분이 가장 이해하기 어려웠다. 이 부분을 이해해야 비로소 점화식을 대략 세울 수 있고, 동적프로그래밍으로 코딩할 준비를 갖출 수 있게된다. 내가 작성한 코드는 아래와 같다. 
```python
number, weight_limit = map(int, input().split())
arr = [[0 for i in range((weight_limit+1))] for j in range(number+1)]
    
for i in range(number):
    weight, value = map(int,input().split())
    for j in range(weight_limit+1):
        if j - weight < 0:
            new_value = 0
        else:
            new_value = arr[i][j - weight] + value
        arr[i+1][j] = max(arr[i][j], new_value)
#     print(arr[i+1])
print(arr[-1][-1])
```
이 문제를 재귀적으로 풀어서 속도를 굉장히 높일 수 있는 방법도 있는데, 그 방식은 너무 어렵고 이해가 안간다.



<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>