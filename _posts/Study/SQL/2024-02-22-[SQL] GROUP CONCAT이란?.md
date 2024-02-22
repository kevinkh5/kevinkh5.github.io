---
title: '[SQL] GROUP_CONCAT 이란?'
author: baduk
date: 2024-02-22 22:16:00 +0900
categories: [Study, SQL]
tags: [SQL]
---

GROUP_CONCAT은 한 테이블에 동일한 ID가 여러개 있다고 했을 때, 그 각각의 동일한 ID마다 가진 특정 필드의 서로 다른 벨류를 하나로 모아줄 때 사용한다.

```sql
SELECT CART_ID, GROUP_CONCAT(NAME) AS NAMES
FROM CART_PRODUCTS
GROUP BY CART_ID
```

예를들면 아래와 같이 테이블이 있을때

|CART_ID|NAME|
|:---:|:---:|
|12|사과|
|12|바나나|
|12|딸기|

CART_ID로 그룹묶어서 NAME에 대해 GROUP_CONCAT을 적용시키면 아래와 같은 결과를 얻을 수 있다.

|CART_ID|NAME|
|:---:|:---:|
|12|사과,바나나,딸기|

