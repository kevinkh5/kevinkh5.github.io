---
title: '[SQL] DCL(Data Control Language) 데이터 제어어 연습'
author: baduk
date: 2024-07-10 04:49:00 +0900
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

## 모든 권한 주기
<pre><code class="language-sql">
GRANT ALL ON ~ TO ~;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
GRANT ALL ON 학생 TO 김하늘;
</code></pre>
</details>

## 삭제 권한 주기 + 권한 받은 사람도 다른 사람에게 권한 줄 수 있도록 하기
<pre><code class="language-sql">
GRANT DELETE ON ~ TO ~ WITH GRANT OPTION;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
GRANT DELETE ON 강좌 TO 김하늘 WITH GRANT OPTION;
</code></pre>
</details>

## SELECT,INSERT,DELETE 권한 취소시키기
<pre><code class="language-sql">
REVOKE SELECT,INSERT,DELETE ON ~ FROM ~;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
REVOKE SELECT,INSERT,DELETE ON 교수 FROM 임꺽정;
</code></pre>
</details>

## UPDATE 권한 취소시키기 + 권한받았던 사람이 다른 사람에게 부여했던 권한도 연쇄적으로 최소시키기
<pre><code class="language-sql">
REVOKE UPDATE ON ~ FROM ~ CASCADE;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
REVOKE UPDATE ON 수강 FROM 임꺽정 CASCADE;
</code></pre>
참고!
임꺽정이 다른 사람에게 UPDATE를 부여할 수 있는 권한을 취소하고 싶을 때,
<pre><code class="language-sql">
REVOKE GRANT OPTION FOR UPDATE FROM 임꺽정;
</code></pre>를 하면 된다. 하지만 임꺽정의 UPDATE 권한을 취소하면 어차피 자동적으로 임꺽정이 다른 사람에게 UPDATE를 부여할 권한이 없어진다.
</details>

## 되돌리기
<pre><code class="language-sql">
ROLEBACK TO ~;
</code></pre>
<details>
    <summary>사용예시</summary>
<!-- summary 아래 한칸 공백 두고 내용 삽입 -->
<pre><code class="language-sql">
ROLEBACK TO P1;
</code></pre>
참고!
P1은 SAVEPOINT이고 만약 SAVEPOINT 없이 그냥 아래와 같이 입력하면
<pre><code class="language-sql">
ROLEBACK;
</code></pre>
COMMIT하지 않은 것은 모두 취소해서 이전 상태로 되돌린다.
</details>


