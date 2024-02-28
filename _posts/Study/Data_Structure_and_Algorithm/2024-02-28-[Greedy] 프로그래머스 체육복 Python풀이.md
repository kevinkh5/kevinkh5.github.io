---
title: '[Greedy] 프로그래머스 체육복 Python풀이'
author: baduk
date: 2024-02-28 14:29:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, python, problem solving, algorithm]
use_math: true
---
출처:
<https://school.programmers.co.kr/learn/courses/30/lessons/42862>

## 문제 간단소개
체육복 도둑맞은 학생에게 체육복을 최대한 빌려주도록 하여 가능한 많은 학생이 체육수업에 참여할 수 있도록하라. 조건은 체육복을 빌려줄 수 있는 사람은 도둑맞은 사람이 아니면서 여벌옷을 가진 사람이고, 그 여벌옷을 가진 사람은 자신의 신체 사이즈 보다 -1 또는 +1인 학생에게만 빌려줄 수 있다.

## 문제 접근 및 풀이
이 문제를 볼 때, 조건을 잘 봐야 한다. 가장 주목해서 봐야 할 조건은 바로 여벌 체육복을 가져온 학생도 도난당했을 수 있다는 점이다. 이 학생의 경우 다른 학생한테 체육복을 빌려주지 않고 본인이 가져온 옷 그대로 입어야 한다. 따라서 이 부분에 대한 처리도 코드에 담겨야 할 것이다.

그리고 체육복을 빌려줄 때도 무언가 규칙이 필요할 것이다. 만약 reserve에 있는 즉, 여벌옷을 가진 학생들 중에서 번호가 낮은 학생부터 순회를 한다고 했을 때 여벌옷을 가진 학생보다 -1인 학생이 있는지를 살펴보고 없으면 오른쪽을 살펴보면서, 왼쪽 우선으로 빌려주도록 해야할 것이다.

예를들어 아래와 같이 1번,3번 학생은 도둑맞았고, 2번,4번 학생이 여벌옷을 가지고 있다고 했을 때, 여벌 옷을 가진 학생이 자신 보다 +1인 학생에게 우선적으로 체육복을 빌려줄 경우 1번 학생은 결과적으로 체육복을 못 빌리게 된다.

<img src ='https://lh3.googleusercontent.com/pw/AP1GczNnYroJYwY_Pz64zama0-eLtOm6gHMVX9emuBHZcQ-OReaAiSHPLRls5bbbvI-ym9L5lho_aJQyfMRszXm6eBiBwfkv-OboYmvvvep2qGok-Usxa1TZ8D9EXyEFOmaypiCFFkgltBVECIcZVqcjDV9x=w844-h520-s-no-gm?authuser=0' width=500 alt='exam'>

여기서 잊지말아야 할 중요한 조건은 여벌옷을 가진 학생들중 번호가 낮은 학생부터 순회를 한다는 조건이 깔려있다. 만약 여벌옷을 가진 학생들중 번호가 큰 순서 먼저 순회한다면 4번학생은 5번학생이 없으므로, 3번 학생에게 체육복을 빌려주고, 2번학생이 3번학생을 빌려줄려고 봤더니 이미 빌려줬으니 1번학생한테 빌려줘서 결국엔 1번, 3번 학생 모두 체육복을 빌릴 수 있게 될 것이다.

따라서 먼저 여벌옷을 가진학생들중 번호가 낮은학생부터 순회할 것인지 또는 번호가 큰 학생부터 순회할 것인지를 정하는게 우선일 것이다. 만약 여벌옷을 가진 학생들중 번호가 낮은 학생부터 순회를 한다는 조건이 깔려있다면 꼭 자신보다 -1인 학생먼저 빌려줄 수 있는지 확인하고 우선적으로 빌려줘야 한다. 그럼 결과적으로 아래와 같이 체육복을 도둑맞은 학생들에게 최대한 빌려줄 수 있게 될 것이다.

<img src ='https://lh3.googleusercontent.com/pw/AP1GczPKLAy87grIxzeMZe_CC6-gT730fsi2uDhCOyIGDnF76FcVOXG4ljcig7amQlI0oAxqSJa-44iNpJ1pOkBcNgrYDdq-2PZ_SZvI0Ed7-1K2gHpx8j_idUgTrf5vJZRmgLWdMrNrogJZrnbvkM8HkDog=w796-h618-s-no-gm?authuser=0' width=500 alt='exam'>

## 제출 코드
```python
def solution(n, lost, reserve):
    # 여벌옷이 있는 학생이 도난 당한 경우에 대한 전처리
    lost_ = set(lost) - set(reserve)
    reserve_ = set(reserve) - set(lost)
    
    lost = lost_
    reserve = reserve_
    
    for r in reserve:
        if r-1 in lost:
            lost.remove(r-1)
        elif r+1 in lost:
            lost.remove(r+1)
    return n - len(lost)
```

원래는 본인보다 번호가 -1인 학생을 먼저 확인하려면 여벌옷을 가진 학생중 번호가 낮은 학생부터 순회해야 하므로 오름차순 정렬을 해줘야한다. 하지만 for in을 사용하여 set에 있는 값을 꺼내올때 작은수부터 꺼내오게 되어서 따로 정렬할 필요가 없어졌다.