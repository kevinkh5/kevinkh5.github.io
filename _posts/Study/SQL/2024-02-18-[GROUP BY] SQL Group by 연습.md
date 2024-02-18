---
title: '[GROUP BY] SQL Group by 연습'
author: baduk
date: 2024-02-18 20:50:00 +0900
categories: [Study, SQL]
tags: [SQL]
---

## 특정자리부터 수를 0으로 만들거나 버리기 TRUNCATE(PRICE,-4)
-4를하면, 72000에서 오른쪽으로부터 4번째 자리인 2부터 오른쪽으로 0으로 만들어줘서 70000이됨.

`TRUNCATE(72000,-4) = 70000`

+1을하면, 소수점 첫째자리 까지 나타내고 다 버림

``TRUNCATE(54.29,+1) = 54.2``


## 소수점 버리기 FLOOR
`FLOOR(54.3) = 54`

## 특정 필드에 대해 그룹으로 묶었을때, 그 필드값이 가장 큰 값을 가진 다른 필드를 다시 그룹으로 묶어서 조회하기

```sql
-- 즐겨찾기가 가장 많은 식당 정보 출력하기
SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM REST_INFO R
WHERE (FOOD_TYPE,FAVORITES) IN (
SELECT FOOD_TYPE, MAX(FAVORITES)
FROM REST_INFO R
GROUP BY FOOD_TYPE
)
ORDER BY FOOD_TYPE DESC
```

## 조건에 맞춰 다르게 값을 보여주기
```
CASE
WHEN ~(조건)~ THEN '보여줄 값'
ELSE '보여줄 값2'
END AS 별명
```
```sql
-- 예시
SELECT 
    employee_name,
    salary,
    CASE 
        WHEN salary >= 80000 THEN 'Executive'
        WHEN salary >= 60000 THEN 'Manager'
        WHEN salary >= 40000 THEN 'Supervisor'
        WHEN salary >= 30000 THEN 'Staff'
        ELSE 'Trainee'
    END AS position
FROM 
    employees;
```

```sql
-- 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기
SELECT C.CAR_ID,
CASE WHEN (C.CAR_ID) IN (
SELECT C.CAR_ID
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY C
WHERE '2022-10-16' BETWEEN C.START_DATE AND C.END_DATE
) THEN '대여중'
ELSE '대여 가능'
END AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY C
GROUP BY C.CAR_ID
ORDER BY CAR_ID DESC
```

## WHERE절에서 서브쿼리 써서 두개 필드 맞춰서 필터하기
WHERE (A,B) IN (SELECT C,D ~~) <- 이와 같은 형식으로

필드 두개를 동시에 가져와서 필터 걸 수 있다.
```sql
-- 식품분류별 가장 비싼 식품의 정보 조회하기
SELECT F.CATEGORY, F.PRICE, F.PRODUCT_NAME
FROM FOOD_PRODUCT F
WHERE (F.CATEGORY, F.PRICE) IN
(SELECT F.CATEGORY, MAX(F.PRICE)
FROM FOOD_PRODUCT F
WHERE F.CATEGORY IN ('과자','국','김치','식용유')
GROUP BY CATEGORY)
ORDER BY PRICE DESC
```

## DISTINCT 적절히 사용하기
디스팅크트를 쓰면 중복된 특정 필드의 값들을 하나로 유니크하게 만들고 집계함수에 적용시킬 수 있다.

아래는 디스팅크트를 사용한 쿼리와 사용하지않은 쿼리다. 년,월,성별에 따라 카운트 집계함수를 사용하여 유저의 명수를 구하고 있는데, 이때 USER_ID필드에 대해 디스팅크트를 사용하지 않으면 중복된 USER_ID까지 집계되어 유저 명수가 크게 나오게된다. 따라서 이때는 디스팅크트를 적절히 사용하여 중복된 값(여기서는 USER_ID)은 합산되지 않도록 해야한다.

```sql
SELECT YEAR(O.SALES_DATE) AS YEAR, MONTH(O.SALES_DATE) AS MONTH, U.GENDER, COUNT(*) AS USERS
FROM USER_INFO U
JOIN ONLINE_SALE O ON U.USER_ID = O.USER_ID
WHERE U.GENDER IS NOT NULL
GROUP BY YEAR, MONTH, U.GENDER
ORDER BY YEAR ASC, MONTH ASC, GENDER ASC
```

```sql
SELECT YEAR(O.SALES_DATE) AS YEAR, MONTH(O.SALES_DATE) AS MONTH, U.GENDER, COUNT(DISTINCT O.USER_ID) AS USERS
FROM USER_INFO U
JOIN ONLINE_SALE O ON U.USER_ID = O.USER_ID
WHERE U.GENDER IS NOT NULL
GROUP BY YEAR, MONTH, U.GENDER
ORDER BY YEAR ASC, MONTH ASC, GENDER ASC
```

## SET으로 로컬변수 선언 및 활용
아래는 SET을 이용해서 로컬변수를 선언한 예시쿼리다. 처음에 hour에 -1을 대입한다. (:= 는 대입연산자) 그리고 hour를 1씩 크게 하면서 반복문 처럼 반복해서 조회하다가 23이 되면 마지막 조회후 종료한다.
```sql
SET @hour := -1; -- 변수선언
SELECT (@hour := @hour + 1) as HOUR,
(SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) as COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23 -- hour가 23까지 반복
```