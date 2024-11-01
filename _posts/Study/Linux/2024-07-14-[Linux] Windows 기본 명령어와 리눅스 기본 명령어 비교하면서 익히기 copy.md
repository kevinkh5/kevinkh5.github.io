---
title: '[Linux] 우분투에서 nohup으로 프로세스 백그라운드에서 실행하기'
author: baduk
date: 2024-11-01 22:31:00 +0900
categories: [Study, Linux]
tags: [Linux]
---

## 파이썬 파일 백그라운드 실행

```shell
nohup python -u myfile.py 1> test_output.log 2>&1 &
```

- nohup ~ &는 백그라운드에서 실행시키며 터미널 창을 꺼도 프로그램은 계속 실행되는 상태를 유지한다. 마지막 &가 백그라운드 실행 명령이다.

- -u옵션은 unbuffered(버퍼링을 비활성화)의 약자로 Python의 출력 버퍼링을 비활성화하는 옵션이다.

- 기본적으로 Python은 표준 출력(stdout)과 표준 오류(stderr)에 대해 버퍼링을 사용한다. 즉, 출력이 바로 기록되지 않고 일정량의 데이터가 쌓일 때까지 기다린 후 출력하는 방식이다. 따라서 -u 옵션을 사용하면 실시간으로 바로바로 로깅 출력이 가능해진다.

- 주의할 점은 출력 버퍼링은 데이터를 일정량 모아서 한 번에 출력하는 방식인데, 이걸 비활성화하면(-u 옵션 사용하면), 속도가 저하될 수 있다고 한다.

**nohup으로 실행시킬 파일은 반드시 755 퍼미션 이라는것 잊지 말자**

참고로 권한은 chmod 명령으로 바꿀 수 있다.


```shell
python myfile.py 1> test_output.log 2>&1
```

이 부분은 리눅스 명령에서 리다이렉션 명령과 관련이 있다.

1은 표준출력, 2는 표준오류를 뜻한다.(여기는 없지만 0은 표준입력)

`myfile.py 1> test_output.log`는 myfile.py을 파이썬에서 실행했을 때 나오는 표준 출력을 test_output.log파일로 리다이렉션, 즉 해당 파일로 기록이 저장된다. 2>&1은 myfile.py의 에러출력을 1(표준출력)이 리다이렉션 하는 경로로 리다이렉션 하겠다는 의미다. 결과적으로 표준출력과 에러 모두 test_output.log 파일에 저장된다.

## 간단하게 아래와 같이 써도 괜찮은듯
```shell
nohup python 4_integration.py 1> output.log &
```

## 실시간 로그 출력보기
```shell
tail -f output.log
```
위 명령으로 실시간 로깅을 볼 수 있다. 단, 처음에 프로세스 실행시 -u 옵션을 줘야 한다.

## 백그라운드에 실행중인 프로세스 끄고 싶다면?
```shell
ps aux | grep myfile.py
```

위 명령으로 ps에 떠있는 많은 프로세스 중 myfile.py에 매칭되는 pid를 확인한다.

```shell
kill <pid>
```
위 명령으로 프로세스를 종료한다.
