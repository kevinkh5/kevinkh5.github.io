---
title: '[Dynamic Programming] LCS문제(Longest Common Subsequence, 최장공통부분서열)확실히 이해해보자'
author: baduk
date: 2024-01-09 10:13:00 +0900
categories: [Study, Data Structre and Algorithm]
tags: [study, Dynamic Programming, DP, python, algorithm]
use_math: true
---
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>

## 처음에 이해하기 힘든 LCS테이블
LCS에서 가장 이해가 안갔던 부분이 테이블 부분 이었다. 특히 가장 이해가 안갔던 부분은 두 문자열을 한 문자씩 추가해 나가면서 같은 것이 있는지 없는지를 판별해나가는데, 서로 같은 문자가 발견된 시점에서 +1이 되어야 하는데 그게 왜 대각선 왼쪽 위에서 구한 LCS값 + 1인지가 이해가 잘 안갔었다. 그리고 서로 다른 문자가 발견된 시점에서는 왜 위의 값과 왼쪽에 있는 값을 비교해서 최대값을 넣어주는지도 잘 이해가 가지 않았다.

이러한 부분들을 아래에서 자세히 생각해보면서 이해해보자.

## 서로 같은 문자가 발견된 시점에서의 빈칸에 대각선으로 왼쪽 위에 있는 값에 + 1을 해서 넣어주는 이유는?
일단 이것을 이해하려면 먼저 대각선 왼쪽 위에 있는 lcs값을 구할 때 비교하는 두 문자열이 무엇인지 생각해봐야 한다. 확실한 것은 대각선 왼쪽 위에 있는 값을 구하기 위한 두 문자열은 대각선 오른쪽 아래에 있는 값을 구하기 위한 두 문자열보다 한 문자씩 적다.

일단 왼쪽 위에 lcs값을 구하기위한 문자열 상태를 떠올려보자. 그리고 오른쪽 아래에 있는 lcs값을 구하기 위한 두 문자열로 바뀌는 순간을 상상해보자. 이 순간에 A가 나오든 B가 나오든 C가 나오든, 무엇이 나오든지 간에 어쨌든 `같은` 문자가 나오면 LCS는 왼쪽 위 lcs값 +1이 될 것이다. 즉. 이게 `문자가 같아지는 순간`에 왜 대각선 왼쪽위에 값에서 + 1을 해준지에 대한 이유다. 아래 그림을 참고해 보자.

<img src = "https://lh3.googleusercontent.com/pw/ABLVV86PoJxSheFCEktlnmBakYOZbE9ET58jqK9dOSxEELGSznw-qNgkAAqVcU054q5oFAbBp3FjN1OLaI0lRsvEldWmLKqNsdWB0FQQJenn5ZqhoV4SKOCW0OOVGrtGqWpnbrLLagoKjWZkXaRdRDE-1aDK=w598-h670-s-no-gm?authuser=0" alt="exam" width="400" height="400">

위에서는 문자가 같을때 +1 처리가 어떻게 이루어지는지에 대한 설명을 했다. 그렇다면 문자가 달라질 때는 어떻게 처리해야 할까? 일단은 문자가 달라지는 경우는 무작정 +1을 해주어서는 안될 것 같은 느낌은 든다. 아래에서 천천히 이해해보자. 

## 서로 다른 문자가 발견된 시점에서의 빈칸에 윗 값과 왼쪽 값을 비교했을 때 큰 값을 넣어주는 이유는? 
테이블에서 CA와 ACAY를 비교하여 LCS구하는 순간 이전에는 두 가지 경우가 있다. 첫번째는 CA와 ACA가 있는 상황(아래 그림에서 1번과 같은 상황)이다.  두번째는 C와 ACAY가 있는 상황(아래 그림에서 2번과 같은 상황)이다.

<img src = "https://lh3.googleusercontent.com/pw/ABLVV85__72pptLRpr4v2ZIdZxmdbB5Poz48DOWdcQXt_fBeaPOTS4-8Z7TFj1TjCaipToJHIQhH8sHDCR1DuNyq12Wby9vKq_TQnlqOUI7chMGBoZ9UnRK7zVWKI9JLpw6qz5oCAWIgnDFe0HqbZUwUjsTf=w1142-h1170-s-no-gm?authuser=0" alt="exam" width="400" height="400">

1번 상황에서 Y가 새롭게 추가된다고 한들 LCS값의 변화가 없을 것이다. 2번 상황도 마찬가지로 A가 새롭게 추가된다고 한들 LCS값의 변화가 없을 것이다. 따라서 1번 상황과 2번상황 중 큰 값이 문자가 달리지는 경우에 대한 LCS값으로 들어간다. 어쨌든간 이 테이블을 채우는 숫자로는 LCS, 즉 `"최장"`공통 부분서열이 들어가야하므로 가능한 최대값을 넣어주어야 하기 때문이다.

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-2582023706445264"
     crossorigin="anonymous"></script>