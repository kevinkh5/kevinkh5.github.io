---
title: '[SQL] DML(Data MANIPULATION Language) 데이터 조작어 연습'
author: baduk
date: 2024-07-10 05:22:00 +0900
categories: [Study, SQL]
tags: [SQL]
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Styled Details</title>
    <style>
        details {
            border: 1px solid #aaa;
            border-radius: 4px;
            padding: 10px;
            background-color: #f9f9f9;
        }
        details[open] {
            background-color: #e6e6e6;
        }
        summary {
            font-weight: bold;
            cursor: pointer;
        }
        pre {
            background-color: #f0f0f0;
            padding: 10px;
            border-radius: 4px;
        }
    </style>
</head>

## 조건에 맞는 튜플 삭제하기
<pre><code class="language-sql">
DELETE
FROM ~
WHERE ~;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
DELETE
FROM 학생
WHERE 이름 = '민수';
</code></pre>
</details>

## 속성에 대한 값이 모두 들어있는 튜플 삽입하기
<pre><code class="language-sql">
INSERT INTO ~
VALUES (~, ~, ~);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
INSERT INTO 학생
VALUES (9816021, '한국산', 3, '경영학개론', '050-1234-1234');
</code></pre>
참고! 굳이 아래와 같이 모든 속성을 다 적을 필요 없다. 어차피 모든 속성의 값을 넣어 삽입할 것이기 때문이다.
<pre><code class="language-sql">
INSERT INTO 학생(학번, 이름, 학년, 신청과목, 연락처)
VALUES (9816021, '한국산', 3, '경영학개론', '050-1234-1234');
</code></pre>
</details>

## 특정 조건에 맞는 튜플 업데이트하기
<pre><code class="language-sql">
UPDATE ~
SET ~ = ~ + ~(변경사항)
WHERE (조건);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
UPDATE 사원
SET 연봉 = 연봉 + 100000
WHERE 직급 = '차장';
</code></pre>
</details>

## 다른 테이블에서 특정조건에 맞는 튜플을 가져와서 삽입하기
<pre><code class="language-sql">
INSERT INTO ~(~,~,~)
SELECT ~,~,~
FROM ~
WHERE (조건);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
INSERT INTO 기획부(성명, 경력, 주소, 기본급)
SELECT 성명, 경력, 주소, 기본급
FROM 사원
WHERE 부서 = '기획';
</code></pre>
</details>

## 하위질의에서 ALL 사용해서 다 가져오기
<pre><code class="language-sql">
SELECT ~, ~
FROM ~
WHERE ~ > ALL (SELECT ~ FROM ~ WHERE (조건));
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
SELECT 제품명, 단가, 제조사
FROM 제품
WHERE 단가 > ALL (SELECT 단가 FROM 제품 WHERE 제조사 = 'H');
</code></pre>
</details>

## 서브쿼리 및 조인 안쓰고 테이블 여러개 가져와서 WHERE 조건으로 처리하기
<pre><code class="language-sql">
SELECT ~.~, ~
FROM ~, ~, ~
WHERE (조건) AND (조건);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
여러 테이블을 가져오면 카티션 곱 수행하는데, 그 중에 WHERE 조건 잘 추가하면 조인쓰지 않고도 가져올 수 있다.
<pre><code class="language-sql">
SELECT 학생정보.학번, 이름, 결제여부
FROM 학생정보, 신청정보, 결제
WHERE 학생정보.학번 = 신청정보.학번
    AND 신청정보.신청번호 = 결제.신청번호
    AND 신청과목 = 'OpenGL';
</code></pre>
참고!
SELECT하고자 하는 속성이 두개 이상의 테이블들이 공통적으로 가지고 있다면, 테이블을 지정해줘야한다.
</details>

## UNION 사용시 주의
유니온 사용해서 데이터 합칠 때, 중복되는 행은 한번만 출력한다.

## NATURAL 조인쓰면 속성 안적고 테이블만 적어도 조인가능하다(단, 둘이 같은 속성을 가지고 있어야 한다)
<pre><code class="language-sql">
SELECT ~, ~
FROM ~ NATURAL JOIN (테이블)
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
JOIN ~ USING 방식
<pre><code class="language-sql">
SELECT 학번, 이름, 학생.학과코드, 학과명
FROM 학생 JOIN 학과
USING(학과코드);
</code></pre>
NATURAL JOIN ~ 방식
<pre><code class="language-sql">
SELECT 학번, 이름, 학생.학과코드, 학과명
FROM 학생 NATURAL JOIN 학과
</code></pre>
</details>

## 왼쪽 테이블을 기준으로 오른쪽 테이블 붙이기 (OUTTER JOIN)
<pre><code class="language-sql">
SELECT ~
FROM ~ a LEFT OUTER JOIN ~ b
ON a.~ = b.~;</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
JOIN ~ USING 방식
<pre><code class="language-sql">
SELECT a.코드, 이름, 동아리명
FROM 사원 a LEFT OUTER JOIN 동아리 b
ON a.코드 = b.코드;
</code></pre>
</details>