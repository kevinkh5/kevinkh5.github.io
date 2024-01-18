---
title: '[Binary Search] 프로그래머스 순위 검색'
author: baduk
date: 2024-01-17 15:02:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
## 문제 간단소개
이 문제는 info라는 인풋으로부터 데이터를 입력받고, query라는 또 다른 인풋을 입력받아 query에 해당하는 데이터와 매치하는 데이터 개수를 출력하는 문제다.


## 문제접근 & 풀이과정

이 문제 풀이는 크게 두가지 과정으로 나눌 수 있을 것 같다. 첫번째는 데이터 삽입 과정이고, 두번째는 데이터 검색 과정이다.

### 데이터 삽입 과정
이 문제에서는 시간복잡도를 줄이기 위해 메모리(리스트)를 사용해야한다.  따라서 여러가지 나올 수 있는 다양한 경우에 대한 쿼리로 접근 할 수 있는 공간을 만들어 주어야 한다. 

이 데이터들을 담을 배열은 4차원 리스트로 만들어야 한다. 그 이유는 문제에서 보면 입력받을 검색에 사용될 필드가 총 4개(언어, 직군, 경력, 소울푸드)이기 때문이다. 점수 필드는 4차원 리스트의 ‘값’으로 들어갈것이기 때문에, 점수필드는 검색에 사용될 필드에서 제외 시켰다.

이렇게 입력 받은 info를 가지고 4차원 리스트에 모든 쿼리의 경우의 수에 대해서 값을 가지고 있으면, 쿼리 하나를 가지고 이 리스트에 저장된 점수의 값들을 리스트 형식으로 가져올 수 있게된다. (어떻게 보면 이 리스트는 작은 DB 같은 느낌이다.)

그리고 쿼리로 이 리스트에 검색해서 가져온 점수들을 이분탐색을 통해서 특정 점수 이상인 데이터의 개수를 알아내야 한다. 일단 이분탐색은 정렬된 수의 대해서 진행할 수 있으므로, 이분탐색을 진행하기 이전에 먼저 수들이 정렬되어 있어야 한다. 

따라서 데이터를 4차원 리스트에 삽입하기 이전에 먼저 info를 sorted key=lambda코드를 사용하여 미리 정렬해놓고 삽입하도록 한다.


### 데이터 검색 과정
이렇게 데이터 삽입 및 정렬이 다 되었으면 쿼리로 이분탐색을 진행하면 된다. 이분탐색을 통해 특정 수 이상이 되는 지점을 찾고, 그 지점으로 부터 인덱스 끝에 대한 사이즈를 구하면 데이터 개수를 구할 수 있다. 최종적으로 각각 쿼리에 대한 구한 데이터 개수를 리스트에 삽입하여 그 리스트를 return해주면 끝이다.

이분탐색할때 리스트에서 찾고자 하는 특정 수 이상을 찾으면 다음 end를 mid + 1이 아닌 그냥 mid로 옮기는데, 그 이유는 end가 특정수의 인덱스를 기억하도록 하기 위함이다. 대신 start는 리스트에서 찾고자하는 특정수 이상의 값이 아니면 mid + 1로 옮기는데, 그 이유는 start를 end와 만나도록 이동시키기 위함이다. 결국 start와 end가 만난 지점이 바로 찾고자하는 특정 수 이상의 값을 가진 인덱스가 된다. start를 리턴해주면 특정수 이상인 값에서 가장 왼쪽 인덱스를 얻을 수 있다.

하지만 여기서 또 주의해야할 케이스가 하나 있는데, 만약 리스트에서 가장 큰 수보다 큰 수를 찾으면 0을 반환하도록 해야하는 부분이다.  따라서 애초에 리스트의 가장큰 수가 담긴 arr[-1]부분을 확인하여 입력받은 값보다 작다면 그냥 0을 return 하도록 한다. 


## 헤맸던 부분
이분탐색 함수에서 헤맸던 부분은 arr가 빈값으로 들어갈 경우에 대해서 처리를 안해줘서 런타임에러가 났다. 빈값으로 들어오면 0을 리턴하도록 바꿔주니 해결됐다.


## 문제풀면서 알아야 하는 부분
이 문제를 풀면서 가장 까다로웠던 부분이 있었는데, 그것은 바로 '-' 표시 부분이었다. '-' 이 표시는 해당 조건을 고려하지 않겠다는 의미인데, 이것 때문에 데이터 삽입과정에서 삽입해야할 경우의 수가 2의 네제곱 즉, 16가지가 되면서 이 부분을 고려해서 데이터를 삽입해야 했다.

이때 필요한 개념이 `비트 연산자 개념`이었다. 아래 코드는 비트 연산자를 이용한 경우의 수를 생성하는 예시 코드다.
```python
def generate_all_permutations(nums):
    n = len(nums)
    total_cases = 1 << n  # 2의 n승
    all_permutations = []

    for i in range(total_cases):
        current_case = []
        for j in range(n):
            if i & (1 << j):
                current_case.append(0)
            else:
                current_case.append(nums[j])
        all_permutations.append(current_case)

    return all_permutations

# 주어진 수열
nums = [1, 2, 3, 4]

# 가능한 모든 경우의 수 생성
all_cases = generate_all_permutations(nums)

# 결과 출력
for case in all_cases:
    print(case)

```
<< 부분은 왼쪽 쉬프트 이동을 뜻하는데, 만약 1<<2 이라고하면 1을 이진수로 바꿔 왼쪽으로 두번 옮기겠다는 뜻이다. 즉 왼쪽으로 두번 옮겼을 때 이진수 100(2)가 된다. 이는 십진수로는 1 곱하기 2의 2승 즉, 4가 된다. (1<<2는 파이썬 코드에서 4로 출력된다.) 또 다른 예시로 2<<2 라고 하면 2를 이진수로 바꿔 10(2)이 되고 이를 왼쪽으로 두번 옮기면 이진수로 1000(2)이고, 십진수로는 2 곱하기 2의 2승, 즉 8이 된다.

 위 예시 코드 함수부분을 간단하게 설명하자면, 함수내에서 n = 4이므로 토탈케이스는 총 16가지가 된다. i는 십진수 0부터 15까지 진행되는데 그 의미는, 이진수로 0000부터 1111까지 가는 것을 의미한다고도 할 수 있다. 이것을 이용하면 크기가 4인 리스트의 모든 부분집합을 구할 수 있다.그 이유는 이진수 0000부터 이진수 1111까지 숫자가 1씩 커지면서 1이 껐다 켜지는 모든 경우의수를 볼 수 있기 때문이다.
 
 안쪽 for문에서는 1을 왼쪽으로 0번쉬프트(1), 한번쉬프트(10), 두번쉬프트(100), 세번쉬프트를(1000) 진행하면서 i와 논리곱(&)을 했을 때 1이 되는 타이밍(1과 1이 만나는 타이밍)에 current_case 리스트에 0을 삽입한다. 즉 1이 켜진 부분에 대한 인덱스에 0을 삽입한다. 그리고 그외 나머지는 nums[j]를 삽입한다. 이런식으로 아래와 같이 0이 들어가는 모든 부분집합에 대한 출력을 얻을 수 있다. 
 ```
[1, 2, 3, 4]
[0, 2, 3, 4]
[1, 0, 3, 4]
[0, 0, 3, 4]
[1, 2, 0, 4]
[0, 2, 0, 4]
[1, 0, 0, 4]
[0, 0, 0, 4]
[1, 2, 3, 0]
[0, 2, 3, 0]
[1, 0, 3, 0]
[0, 0, 3, 0]
[1, 2, 0, 0]
[0, 2, 0, 0]
[1, 0, 0, 0]
[0, 0, 0, 0]
```
 따라서 문제에서 '-'부분에 대한 모든 경우의 수를 만들어 주기 위해서는 비트 연산자를 이용하여 부분집합을 구하면 된다.


## 제출코드
```python
def solution(info_list, query_list):
    def generate_all_permutations(nums):
        n = len(nums)
        total_cases = 1 << n
        all_permutations = []
        for i in range(total_cases):
            current_case = []
            for j in range(n):
                if i & (1 << j):
                    current_case.append(0)
                else:
                    current_case.append(nums[j])
            all_permutations.append(current_case)
        return all_permutations

    def insert_data(info_list):
        info_list = sorted(info_list, key = lambda x: int(x.split()[-1]))
        for info in info_list:
            info = info.split()
            a,b,c,d = dic[info[0]],dic[info[1]],dic[info[2]],dic[info[3]]
            nums = generate_all_permutations([a,b,c,d])
            grade = int(info[-1])
            for num in nums:
                arr[num[0]][num[1]][num[2]][num[3]].append(grade)
    
    def binary_search(arr, n):
        if len(arr)==0 or n > arr[-1]:
            return 0
        start = 0
        end = len(arr) - 1
        while start < end:
            mid = (start + end) // 2
            if arr[mid] >= n:
                end = mid
            else:
                start = mid + 1
        count = len(arr)-start
        return count
    
    arr = [[[[[]for i in range(3)]for i in range(3)]for i in range(3)]for i in range(4)]
    dic = {'-':0,'cpp':1,'java':2,'python':3,'backend':1,'frontend':2,'junior':1,'senior':2,'chicken':1,'pizza':2}
    insert_data(info_list)

    results = []
    for query in query_list:
        query = query.replace(' and','')
        query = query.split()
        a,b,c,d = dic[query[0]],dic[query[1]],dic[query[2]],dic[query[3]]
        grade_list = arr[a][b][c][d]
        grade = int(query[-1])
        result = binary_search(grade_list,grade)
        results.append(result)
    
    return results
```