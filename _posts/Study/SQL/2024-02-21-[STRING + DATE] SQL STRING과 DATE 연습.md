---
title: '[STRING + DATE] SQL STRING과 DATE 연습'
author: baduk
date: 2024-02-21 21:08:00 +0900
categories: [Study, SQL]
tags: [SQL]
---

## 문자열 불이기(CONCAT)

CONCAT을 사용하면 문자열을 원하는대로 붙여서 출력할 수 있다.
```sql
SELECT CONCAT('/home/grep/src/',B.BOARD_ID,'/',F.FILE_ID,F.FILE_NAME,F.FILE_EXT) AS FILE_PATH
FROM USED_GOODS_BOARD B
JOIN USED_GOODS_FILE F ON B.BOARD_ID = F.BOARD_ID
WHERE B.VIEWS IN(
SELECT MAX(B.VIEWS)
FROM USED_GOODS_BOARD B)
ORDER BY F.FILE_ID DESC
```

## 문자열 특정 인덱스부터 출력하기(SUBSTRING)
SUBSTRING(U.TLNO, 1, 3)
이렇게 하면 1번 인덱스부터 3개를 출력하라는 의미다.
(SQL에서는 인덱스 시작이 0부터가 아니라 1부터 임)


## 새롭게 테이블을 선언하기(WITH ~ AS)

아래와 같이 WITH를 사용하면 테이블(RENTAL_HISTORY, TRUCKS)을 새롭게 임시적으로 만들어서 사용할 수 있다. 
```sql
WITH RENTAL_HISTORY AS (
    SELECT *
         , CASE
               WHEN DATEDIFF(END_DATE, START_DATE) + 1 < 7 THEN NULL
               WHEN DATEDIFF(END_DATE, START_DATE) + 1 < 30 THEN '7일 이상'
               WHEN DATEDIFF(END_DATE, START_DATE) + 1 < 90 THEN '30일 이상'
               ELSE '90일 이상'
           END AS DURATION_TYPE
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
), TRUCKS AS (
    SELECT *
    FROM CAR_RENTAL_COMPANY_CAR
    WHERE CAR_TYPE = '트럭'
)
```

## LEFT JOIN vs INNER JOIN(JOIN)

먼저 INNER JOIN의 경우 두 테이블을 교집합식으로 합쳐줄때 사용한다. 하지만 LEFT JOIN은 왼쪽 테이블을 기준으로 오른쪽 테이블을 붙여주는 느낌으로 합친다. 즉, LEFT JOIN을 사용하면 왼쪽테이블의 모든 행을 포함하지만, 오른쪽 테이블과 겹치는 부분이 없는 부분에 대해서는 NULL로 채워진다.
```sql
SELECT H.HISTORY_ID AS HISTORY_ID,
    CASE
        WHEN P.DURATION_TYPE IS NULL THEN ROUND(T.DAILY_FEE * (DATEDIFF(H.END_DATE, H.START_DATE) + 1))
        ELSE ROUND(T.DAILY_FEE * (DATEDIFF(H.END_DATE, H.START_DATE) + 1) * (100 - P.DISCOUNT_RATE)/100)
    END AS FEE
FROM TRUCKS AS T
JOIN RENTAL_HISTORY AS H ON T.CAR_ID = H.CAR_ID
LEFT JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN AS P ON T.CAR_TYPE = P.CAR_TYPE
AND H.DURATION_TYPE = P.DURATION_TYPE
ORDER BY FEE DESC, HISTORY_ID DESC
```
참고로 조인을 사용할때 위 LEFT JOIN과 같이 두 가지 조건을 동시에 걸 수도 있다.