---
title: '[SQL] DDL(Data Define Language) 데이터 정의어 연습'
author: baduk
date: 2024-07-09 05:56:00 +0900
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

## 테이블에 속성 추가하기
<pre><code class="language-sql">
ALTER TABLE ~ ADD ~ VARCHAR(20);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
ALTER TABLE 학생 ADD 주소 VARCHAR(20);
</code></pre>
<pre><code class="language-sql">
ALTER TABLE patient ADD job CHAR(20);
</code></pre>
</details>

## 인덱스 정의하기
<pre><code class="language-sql">
CREATE INDEX ~ ON ~(~);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE INDEX idx_name ON student(name);
</code></pre>
<pre><code class="language-sql">
CREATE INDEX 직원_name ON 직원(이름);
</code></pre>
</details>

## 테이블 만들기 + 제약조건 걸기
<pre><code class="language-sql">
CREATE TABLE ~
	(~ CHAR(10),
	CONSTRAINT ~ CHECK (조건)
	CONSTRAINT ~ FOREIGN KEY(~) REFERENCES ~(~));
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE TABLE patient
    (id CHAR(5) PRIMARY KEY,
    name CHAR(10),
    sex CHAR(1),
    phone CHAR(20),
    CONSTRAINT sex_ck CHECK (sex='f' or sex='m'),
    CONSTRAINT id_fk FOREIGN KEY(id) REFERENCES doctor(doc_id));
</code></pre>
조건 예시:
<pre>
gender = ‘m’ or gender = ‘f’
생년월일 >= ‘1980-01-01’
직책 IN (‘사원’, ‘대리’)
</pre>
</details>

## 테이블 만들기 + NULL값 안들어가게 조건걸기 + 기본키 지정 + 외래키 지정 (+ 참조하는 릴레이션의 튜플을 삭제하거나 갱신할 때 외래키 값을 어떻게 처리할지)
<pre><code class="language-sql">
CREATE TABLE ~
	( ~ CHAR(~) PRIMARY KEY,
	~  CHAR(~) NOT NULL,
	FORIGN KEY(~) REFERENCES ~(~)
		ON DELETE SET NULL
		ON UPDATE CASCADE
	); 
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
PRIMARY KEY 작성방법1
<pre><code class="language-sql">
CREATE TABLE instructor
    (id CHAR(5) PRIMARY KEY,
    name CHAR(15) NOT NULL,
    dept CHAR(15),
    FOREIGN KEY(dept) REFERENCES Department(dept)
        ON DELETE SET NULL
        ON UPDATE CASCADE
    );
</code></pre>
PRIMARY KEY 작성방법2
<pre><code class="language-sql">
CREATE TABLE instructor
    (id CHAR(5),
    name CHAR(15) NOT NULL,
    dept CHAR(15),
    PRIMARY KEY(id),
    FOREIGN KEY(dept) REFERENCES Department(dept)
        ON DELETE SET NULL
        ON UPDATE CASCADE
    );
</code></pre>
참고
<pre>
NO ACTION : 아무것도 안함
CASCADE : 그대로 자동 반영
SET NULL : NULL값으로 업뎃
SET DEFAULT : DEFAULT값으로 업뎃
</pre>
</details>


## 뷰 만들기 + 두 개 릴레이션으로 부터 속성 가져오기 + 조건걸기(WHERE 절)
<pre><code class="language-sql">
CREATE VIEW ~(~,~,~) AS
	SELECT ~.~, ~.~, ~.~
	FROM ~, ~
	WHERE ~.~ = ~.~ ;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE VIEW cc(ccid,ccname,instname) AS
    SELECT Course.id, Course.name, Instructor.instname
    FROM Course, Instructor
    WHERE Course.instructor = Instructor.id;
</code></pre>
</details>

## 테이블 만들 때 외래키지정 두 가지 방법
<pre><code class="language-sql">
CREATE TABLE ~
	(~ NUMBER(4) PRIMARY KEY,
	~ NUMBER(2) FOREIGN KEY REFERENCES ~
	ON DELETE CASCADE
	);
CREATE TABLE ~
	(~ NUMBER(4) PRIMARY KEY,
	~ NUMBER(2),
	FOREIGN KEY ~ REFERENCES ~(~)
	ON DELETE CASCADE
	);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE TABLE 사원
    (사원번호 NUMBER(4) PRIMARY KEY,
    사원명 VARCHAR2(10),
    근무지번호 NUMBER(2) FOREIGN KEY REFERENCES 근무지
    ON DELETE CASCADE
    );

CREATE TABLE 사원
    (사원번호 NUMBER(4) PRIMARY KEY,
    사원명 VARCHAR2(10),
    근무지번호 NUMBER(2),
    FOREIGN KEY(근무지번호) REFERENCES 근무지(근무지번호)
    ON DELETE CASCADE
    );
</code></pre>
</details>

## 인덱스 정의(중복 허용하지 않도록 오름차순으로)
<pre><code class="language-sql">
CREATE UNIQUE INDEX ~ ON ~(~ ASC);
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE UNIQUE INDEX Stud_idx
    ON Student(ssn ASC);
</code></pre>
<pre>
*참고 
UNIQUE INDEX 관하여 알아야 할 것 두 가지!
1.중복허용하지 않도록 인덱스를 정의하려면, 기존의 데이터 역시 중복되지 않아야 한다.
2.만약 기존의 데이터가 중복되지 않아서 UNIQUE로 인덱스를 정의했다면, 이후 중복된 데이터가 들어가지 못한다.
</pre>
</details>

## 도메인 정의 + 제약 사항 + 여기서 주의해야할 것은 “VALUE IN” 사용
<pre><code class="language-sql">
CREATE DOMAIN 직위 VARCHAR2(10) DEFAULT “~”
CONSTRAINT ~ CHECK(VALUE IN(“~”, “~”, … , “~”));
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE DOMAIN 직위 VARCHAR2(10)
DEFAULT "사원"
CONSTRAINT VALID-직위 CHECK(VALUE IN("사원",
"대리","과장","부장","이사","사장"))
</code></pre>
참고!
<pre>
CHECK뒤에 VALUE가 왔는데, 그 이유는 해당 도메인은 아직 생성도 안돼서,
“직위 IN”이라고 쓸 수 없기 때문이다. 존재하지 않는 도메인에 접근시도 한것이니까.
따라서 “직위 IN”이 아니라 “VALUE IN”이라고 적어야 함.
</pre>
정의된 도메인은 어떻게 사용되는가?
바로 아래 테이블 생성시에 그 도메인 이름을 가지고 사용된다.
<pre>
<code class="language-sql">
CREATE TABLE 사원 (
    직원코드 NUMBER NOT NULL,
    성명 CHAR(10) UNIQUE,
    직책 직위,  -- 여기서 도메인 직위를 사용합니다.
    연봉 NUMBER
);
</code>
</pre>
</details>


## 테이블 삭제 + 참조중인거 제거할지 말지
<pre><code class="language-sql">
DROP TABLE ~ CASCADE;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
DROP TABLE 학생 CASCADE;
</code></pre>
</details>

## 테이블 만들기 + 속성조건(중복불가, NULL값 불가, 특정 값만 허용)
<pre><code class="language-sql">
CREATE TABLE ~
	(~ NUMBER NOT NULL,
	~ CHAR(10) UNIQUE,
	~ CHAR(10) CHECK (~ IN(‘~’, ‘~’, … , ‘~’)));
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
CREATE TABLE 사원
    (직원코드 NUMBER NOT NULL,
    성명 CHAR(10) UNIQUE,
    직책 CHAR(10) CHECK (직책 IN('사원','대리','과장','팀장'))
    연봉 NUMBER);
</code></pre>
</details>