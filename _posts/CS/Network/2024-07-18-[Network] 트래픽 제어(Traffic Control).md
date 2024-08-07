---
title: '[Network] 트래픽 제어(Traffic Control)'
author: baduk
date: 2024-07-18 16:08:00 +0900
categories: [CS, 네트워크]
tags: ['네트워크','정보처리기사']
published : true
---
## 트래픽 제어
트래픽 제어는 `전송되는 패킷의 흐름이나 양을 조절하는 기능`으로 네트워크의 보호, 성능 유지, 네트워크 자원의 효율적인 이용을 위해 사용된다.

### 트래픽 제어 종류 3가지
- 흐름 제어(Flow Control)
- 폭주(혼잡) 제어(Congestion Control)
- 교착상태 방지

## 흐름 제어(Flow Control)
흐름 제어는 트래픽 제어의 한 방법으로 `송수신 측 사이에` 전송되는 패킷의 양이나 속도를 규제하는 기능이다. 흐름제어에는 두 가지 기법이 있는데, 하나는 정지-대기(Stop-and-Wait), 다른 하나는 슬라이딩 윈도우다.

정지-대기 기법의 경우 수신 측의 ACK신호를 받은 후에 다음 패킷을 전송하는 방식이고, 반면 슬라이딩 윈도우 기법은 정지-대기 기법과 달리 매번 ACK를 기다리지 않고, ACK신호 없이 보낼 수 있는 패킷의 최대치(윈도우 사이즈)를 정해서 여러 패킷을 연속해서 송신하는 방식이다.(윈도우 사이즈는 상황에 따라 변할 수 있다.) 따라서 슬라이딩 윈도우의 경우 정지-대기 방식보다 전송 효율이 좋다.


## 폭주(혼잡) 제어(Congestion Control)
바로 위에서 언급한 흐름 제어는 `송수신 측 사이의 패킷 수`를 제어하는 기능이지만, 폭주 제어는 `네트워크 내의 패킷 수`를 조절하는 기능으로 네트워크 오버플로우를 방지한다. 폭주 제어에는 두 가지 기법이 있는데 하나는 느린 시작(Slow Start)이고 다른 하나는 혼잡 회피(Congestion Avoidance)이다.

느린 시작 기법의 경우 Congestion Window의 사이즈를 1부터 두 배씩 증가시켜가면서 진행되는데, 처음에는 느리지만 갈 수록 빨라진다. 이후 전송 데이터의 크기가 임계 값에 도달하게되면 혼잡 회피 단계로 넘어간다.

느린 시작 단계에서 혼잡회피 단계로 넘어가면 Congestion Window 사이즈를 1씩 선형적으로 증가시켜 혼잡을 예방한다.