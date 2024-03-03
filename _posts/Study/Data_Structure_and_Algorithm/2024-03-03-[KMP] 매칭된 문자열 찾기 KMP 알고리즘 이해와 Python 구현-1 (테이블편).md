---
title: '[KMP] 매칭된 문자열 찾기 KMP 알고리즘 이해와 Python 구현-1 (테이블편)'
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

그리고나서 i는 또 한칸 이동하여 인덱스 7을 가리키고 j는 1을 가리키고 있는데, i,j 모두 동일한 문자(a)를 가리키고 있으므로 j = j+1로 업데이트 되고, arr[i]는 업데이트 된 j, 3으로 업데이트 된다.

그리고 i는 마지막 인덱스까지 왔으므로 종료된다.

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

여기까지만 해도 내용이 벌써 좀 길어진 것 같다. 지금까지 KMP 알고리즘 사용을 위한 테이블에 대해서 알아보았고, KMP 알고리즘 사용에 대해서는 2장에 이어서 진행하겠다.