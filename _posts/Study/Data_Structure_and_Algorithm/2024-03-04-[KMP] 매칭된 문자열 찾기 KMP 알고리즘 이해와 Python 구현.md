---
title: '[KMP] 매칭된 문자열 찾기 KMP 알고리즘 이해와 Python 구현'
author: baduk
date: 2024-03-03 19:11:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---

## KMP 알고리즘이란?
KMP 알고리즘은 매칭된 문자열의 인덱스를 알아내기 위한 알고리즘이다. KMP 알고리즘을 사용하는 이유는 브루트포스 방식으로 매치되는 문자열을 찾을 때 발생되는 높은 시간복잡도를 해결하기위해 사용한다. 브루트포스방식으로 매치되는 문자열을 찾을 때 시간복잡도는 O(N*M)으로 굉장히 높지만, KMP알고리즘은 O(N+M)의 시간복잡도를 가지므로 상대적으로 굉장히 효율적이라고 할 수 있다.

## KMP 알고리즘 이해하기

### KMP 접두사, 접미사 길이 테이블
KMP 알고리즘을 배우기 이전에 먼저 접두사 접미사에 대해서 알아야 한다. 접두사는 앞에 붙어있는 문자, 접미사는 뒤에 붙어있는 문자를 뜻하며, KMP 알고리즘은 접두사이면서 동시에 접미사인 문자열의 길이 정보를 가지고 진행되기 때문에 KMP알고리즘 이해를 위해 이 부분을 먼저 알아야 한다.

따라서 아래에서 접두사이면서 접미사인 문자열의 길이를 구하는 예시를 들어보겠다. 아래 예시는 문자열 ABACAABA을 index 0부터 시작해서 문자열의 끝까지를 탐색해가면서 접두사이면서 접미사인 문자열의 길이를 구하는 과정이다.

```
A
한 글자는 접두사이면서 접미사인 최대 문자열이 없음 -> 문자열 길이 0

AB
접두사이면서 접미사인 문자열 없음 -> 문자열 길이 0

ABA
접두사이면서 접미사인 문자열 = A -> 문자열 길이 1

ABAC
접두사이면서 접미사인 문자열 없음 -> 문자열 길이 0

A BAC A
접두사이면서 접미사인 문자열 = A -> 문자열 길이 1

A BACA A
접두사이면서 접미사인 문자열 = A -> 문자열 길이 1

AB ACA AB
접두사이면서 접미사인 문자열 = AB -> 문자열 길이 2

ABA CA ABA
접두사이면서 접미사인 문자열 = ABA -> 문자열 길이 3
```

이런식으로 ABACAABA 문자열의 접두사이면서 접미사인 문자열의 길이를 모두 구해 리스트에 넣으면 table 리스트 안에는 아래와 같이 들어갈 것이다.

table = [0, 1, 0, 1, 1, 2, 3]


### 테이블을 만들 때 알고리즘 동작과정
접두사이면서 접미사인 문자열의 길이를 담을 테이블(앞으로 줄여서 접접테이블이라고 하겠다.)을 만들기 위한 동작 과정은 아래와 같다.

먼저 문자열의 사이즈만큼의 테이블을 0으로 초기화한다. 그리고 j=0, i=1 으로 초기화하여 i를 1칸씩 이동하면서 진행한다.

아래 그림에서는 a b a c a a b a 문자열의 접접테이블을 만드는 과정을 나타낸다.

### 과정1 (i=1, j=0) & (i=2, j=0)
처음 j=0, i=1 일때 i, j 각각이 가리키는 문자가 다르므로 arr[i]는 0을 채운다.

그리고 i는 한칸 이동하여 인덱스2를 가리킨다. 현재 i가 한칸 이동하여, i=2, j=0인 상태인데, 이때 i,j 각각이 가리키는 문자가 a로 동일하므로 j를 한칸 +1 이동시키고 arr[i]는 그 j를 받아서 1로 업데이트 된다.

<img src='https://lh3.googleusercontent.com/pw/AP1GczPqGXQjY7gG13cQ9fbURiykQZmWI8vXtjSFpu3OSjtYF6NtIPdSmG4hXLDNlKDelV7OT-9LUlFp5cglWTDBrFsgmiK0CrgDEk6BNV-fdapkkBCuJEfyXzd9Ik-wkAYadu1m8YqoDFSGT-1-GJdcaJ58=w866-h1116-s-no-gm?authuser=0' alt='exam' width=500>

### 과정2 (i=3, j=1) & (i=4, j=0)
i는 또 이동하여 3을 가리켜 현재 i=3, j=1 이다. 이 때 j는 b를 가리키고 i는 c를 가리켜 서로 다른 문자열을 가리키고 있다. 이때는 j가 가리키고 있는 인덱스의 전 인덱스가 가리키는 값을 j로 업데이트 한다. 그러면 j는 0으로 이동하게 된다.

그리고 i가 또 한칸 이동하여 4를 가리키고 현재 i=4, j=0인 상태인데, 이때 i,j가 모두 a라는 동일한 문자를 가리키고 있으므로 j를 +1 칸 이동시키고, arr[i]=j로 업데이트 한다.

<img src='https://lh3.googleusercontent.com/pw/AP1GczNijryLyfTE4_oW2uS3WIP18jgeSMUHV6ueHCAsAAqoULqTgtFqPUywqjPtabawREHtApuUezt5NjPYLb1WxKUzJGM9bnYcpa6EhkJNAEEXiFMMfW4GPSCj8775nrS-sB00tHh6CHexVzi6JoSGW2Jg=w962-h1292-s-no-gm?authuser=0' alt='exam' width=500>

### 과정3 (i=5, j=1)

i는 또 한칸 이동하여 5를 가리키고 j는 1을 가리키고 있다. 이때 현재 i,j가 각각 가리키고 있는 문자는 다르므로 j가 가리키고 있는 인덱스의 전 인덱스가 가리키는 값을 j로 업데이트 한다. 그러면 j는 0으로 이동하게 된다.

이때 j가 0으로 이동하고나서 i,j가 각각 가리키고 있는 값이 모두 a로 동일하므로 j를 다시 +1 칸 이동한다. 그리고 +1 칸 이동된 j로 arr[i]를 업데이트 한다.
<img src='https://lh3.googleusercontent.com/pw/AP1GczNzTP5FXzwuOCd4Y4_QUJTXO1sZluRXhVii-7MwA3SRhjNNaZNBqYCQ9ZiJtsbbJEo4XMo5LJsrAqF3mJM5cfTZXlFtjN_hS5UgyBpK6DWWwX3jrHU-d4Pv6cvz4w3eEaWH_zmV7_RfTPaEuiEBdZWF=w984-h850-s-no-gm?authuser=0' alt='exam' width=500>


### 과정4 (i=6, j=1) & (i=7, j=2)

이제 마지막 과정이다. i는 또 한 칸 이동하여 6을 가리키고 j는 1을 가리키고 있다. 이때 i,j둘다 가리키고 있는 문자가 b로 동일하다. 따라서 j = j+1 로 업데이트되고, arr[i]는 업데이트된 j로 업데이트 된다. 따라서 2가 들어간다.

그리고나서 i는 또 한칸 이동하여 인덱스 7을 가리키고 j는 2를 가리키고 있는데, i,j 모두 동일한 문자(a)를 가리키고 있으므로 j = j+1로 업데이트 되고, arr[i]는 업데이트 된 j, 3으로 업데이트 된다.

그리고 i는 마지막 인덱스까지 왔으므로 종료되고, 최종적으로 테이블에 [0 , 0 , 1 , 0 , 1 , 1 , 2, 3]에 담기게 된다.

<img src='https://lh3.googleusercontent.com/pw/AP1GczM_No9HzC3WOZorzResjousUEn2GObAlFFlYvtX56W3VgVPmQy6CK6nQxKFqBroHUkOjDMPRt30jeF1NcFweFzPPY_O_ocfN8pFGg-cONg1oSquvm7zyxJF1_aVxb8DrtWTYtYv09dLnVtEVpDFN_zi=w968-h1220-s-no-gm?authuser=0' alt='exam' width=500>


이와 같이 위에서 접접테이블을 만드는 알고리즘 동작과정을 설명하였다. 접접테이블을 만드는 파이썬코드는 아래와 같다. 사실 위 동작과정을 글로 서술된 것을 보고 이해하는 것은 굉장히 힘들 것이다. 동작과정의 이해를 돕기위해 아래 코드를 디버깅모드로 한스텝씩 진행해보는것을 추천한다.

## 접접 테이블(접두사이면서 접미사인 문자열의 길이를 담는 테이블) 만드는 파이썬 코드
```python
def kmp_table(pattern):
    pattern_size = len(pattern)
    # 접두사이면서 접미사인 문자열의 길이를 담을 리스트 초기화
    arr = [0 for _ in range(pattern_size)]
    # 처음에 j = 0으로 초기화하고 i를 1부터 순회하면서 진행
    j = 0
    for i in range(1, pattern_size):
        while j > 0 and pattern[j] != pattern[i]:
            j = arr[j - 1]
        # i와 j가 가리키는 값이 같으면 j는 +1 증가 시키고
        # j의 인덱스를 i가 가리키는 리스트의 값으로 업데이트
        if pattern[i] == pattern[j]:
            j += 1
            arr[i] = j
    return arr
```

---

## 접접 테이블을 KMP알고리즘에서 이용하기

이제 접접 테이블을 만들었으면, KMP 알고리즘에서 이용해보자.

예를들어 C A B A B A B A B B 라는 본문이 있고, A B A B B 라는 단어를 검색한다고 해보자.

동작과정은 다음과 같다.

### 과정1 (i=0, j=0)

본문을 가리키는 i = 0, 검색어를 가리키는 j = 0 이라고 초기화하고 시작한다. i=1, j=1일 때, 서로 가리키는 문자가 다르므로 그냥 반복문 기본 플로우대로 i = i+1 로 업데이트한다.

<img src="https://lh3.googleusercontent.com/pw/AP1GczM-7XvgGlhMaiiW__5GiCSo2f9OSEQpBU31t_m4yZCOG5NHShwNxVNN3AUlWmX5vrqdzhi75KwmAWPIw94J42AguFLQ0wEyMKaIpGO2qHQF9RtOh9OAU8CtLfsHKvbke7KWIh9U6TvYXyFk64UzpYXK=w878-h488-s-no-gm?authuser=0" alt='exam' width=500>


### 과정2 (i=1, j=0)

i=1, j=0일 때, 서로가리키는 문자가 같고 그 뒤에 이어서 i와 j를 1씩 증가시켜가며 비교한다. 증가시켜가며 비교하다가, i=5, j=4 지점에서 문자 비매칭이 발생한다. 이때 j는 비매칭 지점 전 인덱스를 가지고 접접테이블에 접근하여 그 값을 가져온다. 그리고 그 접접 테이블로 부터 가져온 그 값 2를 가지고 검색어의 탐색 시작 지점인 변수 j를 업데이트한다.

<img src="https://lh3.googleusercontent.com/pw/AP1GczMaiFn4oN5SUib5_NOAPGxIK0Sv3nsk3mNt0H4uO_nGTZ1_2k70xpsSe8FmSxNcEsnvmxfQMZUAF5eu3C7K5lfCYUesFTzRDsoBTRsS5d7WxxSSux2u-A0GotKlSNPoDitd4ESIBHDb-uoBZ4f5ddA3=w876-h666-s-no-gm?authuser=0" alt='exam' width=500>


### 과정3 (i=5, j=2)

비매칭 지점 발견 후 업데이트 직후의 대해서 i,j는 아래와 같이 가리키고 있을 것이다.

<img src="https://lh3.googleusercontent.com/pw/AP1GczPYbuGL5EEEfgNAkFbiwTtZzXwJL2dBEjfX-Nkdoo901YjDBZ0AlN9V7ZKpwxVsnrjSkvksmgnJC4HATuVfQ006DWD85vdC0Uu3MrIYiHLU9rKhDEQ_deHK8OdJyoyDT0mlNSAT6VR7YcyhMcJLvwkB=w958-h400-s-no-gm?authuser=0" alt='exam' width=500>

이 과정에서 우리는 KMP알고리즘이 기존의 브루트포스 방식과 비교했을때 반복과정이 훨씬 더 적다는 사실을 알 수 있다. 즉, 브루트포스 방식으로 진행했다면, 다음 루프에서 i는 2로 되돌아가고 j는 0부터 시작하면서 비교 반복을 했을 텐데, KMP 알고리즘의 경우 i는 선형적으로 이동하니까 i는 6으로 이동하게 될 것이고, j는 0부터 시작이 아니라 3부터 시작을 하게된다. 브루트포스 방식에서 진행했다면 훨씬 많이 발생했을 비교 반복 횟수를 KMP알고리즘은 획기적으로 줄인 셈이다.

이 처럼 j인덱스를 스마트하게 업데이트 해가면서 매칭되는 문자열을 찾는 알고리즘이 바로 KMP알고리즘이다.

## KMP알고리즘 파이썬 코드
```python
def kmp(word, pattern):
    table = kmp_table(pattern)
    results = []
    j = 0 # j는 검색어를 가리키는 인덱스
    # i는 긴 문자열(검색대상)을 가리키는 인덱스
    for i in range(len(word)):
        while j > 0 and word[i] != pattern[j]:
            j = table[j - 1]
            # 1개 이상 매칭되다가 비매칭 지점 만나면, j는 테이블을 j-1인덱스로 참조해서 가져온 값으로 초기화
            # 비매칭 지점에서 땡겨오는 부분
        # 현재 인덱스에서 값이 일치하면, j(검색어)를 +1 증가
        if word[i] == pattern[j]:
            if j == len(pattern) - 1:
                results.append(i - j)
                j = table[j] # 검색어를 완전히 찾았다면 j를 테이블을 참조하여 접두사,접미사에 따라 적절히 초기화
                # 완전 매칭 이후 땡겨오는 부분
            else:
                j += 1
    return results
```

