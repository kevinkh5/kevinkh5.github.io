---
title: '[Network] TCP/IP의 구조'
author: baduk
date: 2024-07-17 19:40:00 +0900
categories: [CS, 네트워크]
tags: ['네트워크','정보처리기사']
published : true
---
## TCP/IP의 구조
OSI 7 Layer의 경우 7개의 계층으로 나누었다면, TCP/IP 구조의 경우, 7 계층을 적절히 묶어 4가지 계층으로 나눈다.
### 1.응용 계층
- 응용 계층
- 표현 계층
- 세션 계층
- ex. HTTP, FTP, DNS, SMTP

### 2.전송 계층
- 전송 계층
- ex. TCP, UDP, RTCP

### 3.인터넷 계층
- 네트워크 계층
- ex. IP, ICMP, IGMP, ARP, RARP

### 4.네트워크 엑세스 계층
- 데이터 링크 계층
- 물리 계층
- ex. Ethernet, IEEE 802, HDLC, X.25, RS-232C, L2TP


## 인터넷 계층 주요 프로토콜

ICMP(Internet Control Message Protocol)
- 인터넷 계층(or 네트워크 계층)의 주요 프로토콜로 IP의 주요 구성원이다.
- 통신중에 발생하는 오류의 처리와 전송 경로 변경등을 위한 제어 메시지를 관리한다.
- 관련 도구로 traceroute, ping이 있고, ping of death와 같은 네트워크 공격 기법에 활용되기도 한다.

ARP(Address Resolution Protocol)
- IP주소를 MAC 주소(물리주소)로 바꾼다.
- 그 반대는 RARP(Reverse Address Resolution Protocol).


## 네트워크 엑세스 계층 주요 프로토콜
L2TP(Later 2 Tunneling Protocol)
- 네트워크 엑세스 계층 중 하나인 데이터 링크 계층의 프로토콜 중 하나이다.
- 터널링 프로토콜 PPTP와 VPN 구현에 사용되는 L2F의 장점을 결합해서 만든 프로토콜이다.
- 자체 암호화 기능이 없어 보통 다른 보안 프로토콜과 함께 사용된다.

