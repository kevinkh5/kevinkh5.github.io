---
title: '[Two-Pointer] 효율적으로 부분 연속 수열의 합 구하는 알고리즘 두포인터 Python 구현'
author: baduk
date: 2024-03-09 15:25:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---

## 두 포인터 알고리즘
두 포인터 알고리즘은 왜 사용할까?

예를들어 특정한 합을 가지는 부분 연속 수열 찾기 문제를 푼다고 해보자.

[1, 2, 3, 4, 5]

예를들어 위 수열에서 구간합이 5인 경우를 찾는다고 해보자. 브루트포스방식으로 푼다면, 인덱스 0~0, 0~1, 0~2, 0~3, 0~4, 1~1, 1~2 ...... 3~4, 4~4 이런식으로 모든 구간을 찾아볼 수 있을 것이다. 하지만 이렇게 풀 경우 시간복잡도는 $O(n^2)$ 이 될 것이다.

이러한 문제를 시간복잡도를 낮춰 효율적으로 해결하기위해 투포인터 알고리즘을 사용할 수 있다. 투포인터 알고리즘을 사용하면 시간복잡도 $O(n)$ 으로 문제해결이 가능해진다.

## 두 포인터 알고리즘의 원리
투포인터 알고리즘은 이름 그대로 포인터 두 개(Start 포인터, End 포인터)를 사용하여 진행할 수 있다. 포인터를 초기에 어디에 둘 지에 대해서는 문제가 요구하는 조건마다 조금씩 다를 수 있다. 만약 위에서 든 예시인 수열의 구간 합을 찾는다고 했을땐, Start 포인터와 End 포인터 모두 인덱스 0을 가리키게 놓고 시작하면된다.

그리고 그 두 포인터 사이의 구간합이 찾고자 하는 수 보다 작다면 End 포인터를 한칸 증가시킨다. 만약 구간합이 찾고자 하는 수 보다 크다면 Start 포인터를 한칸 증가시킨다. 만약 찾고자하는 수와 구간합이 같다면 카운트를 1 증가시키고 Start포인터를 한칸 증가시킨다.

이와 같이 투포인터 알고리즘을 사용하면 선형적인 복잡도로 찾고자 하는 수에 맞는 구간합의 개수를 구할 수 있다.

## 두 포인터 파이썬 구현

```python
# n은 리스트의 크기, m은 찾고자 하는 구간 합
n,m = map(int,input().split())
arr = list(map(int, input().split()))
cnt = 0
interval_sum = 0
end = 0
for start in range(n):
    while interval_sum < m and end < n:
        interval_sum += arr[end]
        end += 1
    if interval_sum == m:
        cnt += 1
    interval_sum -= arr[start]
print(cnt)
```