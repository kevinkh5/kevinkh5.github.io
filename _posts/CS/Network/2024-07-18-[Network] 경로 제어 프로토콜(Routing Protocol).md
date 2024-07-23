---
title: '[Network] 경로 제어 프로토콜(Routing Protocol)'
author: baduk
date: 2024-07-18 15:39:00 +0900
categories: [CS, 네트워크]
tags: ['네트워크','정보처리기사']
published : true
---
## 경로 제어 프로토콜(Routing Protocol)
### IGP(Interior Gateway Protocol, 내부 게이트웨이 프로토콜)
#### RIP(Routing Information Protocol, 거리 백터 라우팅 프로토콜)
- 가장 널리 사용되는 라우팅 프로토콜로 소규모 네트워크에서 사용된다.
- 벨만포드 알고리즘이 사용된다.
- 최대 홉(Hop)수가 15로 제한된다.(Hop은 중간에 거쳐지는 네트워크 수를 의미한다)
- 라우팅 정보를 30초마다 네트워크 내의 모든 라우터에 알리고, 180초이내에 새로운 라우팅 정보가 수신되지 않으면 해당 경로를 이상 상태로 간주한다.

#### OSPF(Open Shortest Path First Protocl)
- RIP의 단점을 해결하여 새로운 기능을 지원하는 프로토콜로 대규모 네트워크에서 사용된다.
- 다익스트라 알고리즘이 사용된다.
- 최단 경로 선정을 위해 라우팅 정보에 노드간 거리정보, 링크상태 정보를 실시간 반영한다.
- RIP와 달리 시간제한 없이 라우팅 정보에 변화가 생길 경우에 변화된 정보만 네트워크 내의 모든 라우터에 알린다.

### EGP(Exterior Gateway Protocol, 외부 게이트웨이 프로토콜)
- 자율 시스템(Autonomous System: 하나의 도메인에 속하는 라우터들의 집합)간의 라우팅, 즉 게이트웨이 간의 라우팅에 사용되는 프로토콜

### BGP(Border Gateway Protocol)
- 자율 시스템간의 라우팅 프로토콜로, EGP의 단점(라우팅 정보의 제한된 전파, 정책 기반 라우팅의 부족, 확장성 문제, 라우팅 루프 방지의 한계)을 보완한 프로토콜
- 초기에 BGP 라우터들이 연결될 때에는 라우팅 테이블을 교환하고 이후에는 변화된 정보만 교환한다.