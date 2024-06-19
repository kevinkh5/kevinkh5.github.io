---
title: '[Web] URL 단축 서비스에서 Http Status 코드 301과 302 중 어떤것을 선택할까'
author: baduk
date: 2024-06-20 00:07:00 +0900
categories: [Study, Web]
tags: [Web]
---

## 301
정리하면 301 의 경우 redirection URL을 브라우저의 캐시에 저장하여 매번 redirection 과정을 거치지 않고 즉시 URL에 접근할 수 있다.


## 302
302의 경우 매번 redirection 과정을 거친다는 차이가 있다.

## 결론
따라서 HTTP Status Code 301을 사용하게 된다면, 첫 번째 요청만 단축 URL 서버로 전송될 것이기 때문에 서버 부하를 줄이는 것에 용이할 것이고, HTTP Status Code 302를 사용하게 된다면, 클릭 발생률이나 발생 위치를 추척하는 것에 좀 더 유리하므로 트래픽 분석에 도움이 될 것이다.