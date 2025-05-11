---
title: '[Python] 파이썬으로 Mysql 다루기(pymysql)'
author: baduk
date: 2024-12-08 16:33:00 +0900
categories: [Programming Language, Python]
tags: [Python, 파이썬]
---

## 임포트 pysql & DB정보 가져오기
```python
import pymysql
import os
from dotenv import load_dotenv
load_dotenv()
DB_HOST = os.getenv('DB_HOST')
DB_USER = os.getenv('DB_USER')
DB_PW = os.getenv('DB_PW')
DB_NAME = os.getenv('DB_NAME')
```

## 나눠서 넣을 때 유용한 함수(메모리 효율적으로 배치를 나눔)
```python
def batch_generator(items, batch_size):
    for i in range(0, len(items), batch_size):
        yield items[i:i + batch_size]
```

## Insert many
```python
try:
    connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
    with connection.cursor() as cursor:
        sql = """
            INSERT INTO table_name (col1, col2, col3) 
            VALUES (%s, %s, %s)
        """
        data = [
            ('value1_1', 'value1_2', 'value1_3'),
            ('value2_1', 'value2_2', 'value2_3'),
            ('value3_1', 'value3_2', 'value3_3'),
        ]
        # 여러 행 삽입
        cursor.executemany(sql, data)
        connection.commit()  # 변경사항 커밋
        print(f"{cursor.rowcount} rows inserted successfully.")
finally:
    connection.close()  # 연결 종료
```
**커넥션을 생성하고 사용이 끝나면 반드시 close를 해야 한다. close를 안해주고 끝낼 경우, 다른 사람이 쿼리로 해당 테이블에 접근을 못할 수도 있다.**

## Insert many(나눠서 넣기)
```python
data_to_insert = [(item['col1'], item['col2'], item['col3']) for item in data_list]
try:
    connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
    with connection.cursor() as cursor:
        for batch in batch_generator(data_to_insert,1000):
            sql = """
                INSERT IGNORE INTO table_name (col1, col2, col3) 
                VALUES (%s, %s, %s)
            """
            cursor.executemany(sql, batch)
            connection.commit()
finally:
    connection.close()
```

## Select 
```python
try:
    connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
    with connection.cursor() as cursor:
        sql_query = "SELECT * FROM your_table_name LIMIT 10;" 
        cursor.execute(sql_query)
        results = cursor.fetchall()  # 또는 cursor.fetchone()을 사용하여 한 행만 가져오기
        for row in results:
            print(row)
finally:
    connection.close()
```

## Select (json형식으로 가져오기)
```python
try:
    connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
    with connection.cursor() as cursor:
        cursor = connection.cursor()
        sql = """
        select *
        from DB_TABLE
        """
        cursor.execute(sql)
        columns = [item[0] for item in cursor.description] # 칼럼정보에서 칼럼이름만 골라서 가져오기
        Data = pd.DataFrame(cursor.fetchall(),columns=columns)
finally:
    connection.close()
```

## Update
```python
try:
    connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
    with connection.cursor() as cursor:
        sql = """
        UPDATE your_table_name
        SET column_name = %s
        WHERE condition_column = %s
        """
        new_value = 'Updated Value'
        condition_value = 'Condition Value'
        cursor.execute(sql, (new_value, condition_value))
    connection.commit()
except Exception as e:
    print(f"Error occurred: {e}")
finally:
    connection.close()
```

## Updata many
```python
try:
    connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
    with connection.cursor() as cursor:
        sql = "UPDATE your_table SET name = %s, age = %s WHERE id = %s"
        data = [
            ('Alice', 25, 1),  # id가 1인 행의 name과 age 업데이트
            ('Bob', 30, 2),    # id가 2인 행의 name과 age 업데이트
            ('Charlie', 28, 3) # id가 3인 행의 name과 age 업데이트
        ]
        cursor.executemany(sql, data)  # 데이터를 한 번에 업데이트
        connection.commit()  # 변경 사항 커밋
        print("Records updated successfully.")
finally:
    connection.close()  # 연결 종료
```


## Delete many (여러개를 나눠서 삭제하여 DB 부하 줄이기)
```python
connection = pymysql.connect(host=DB_HOST, user=DB_USER, password=DB_PW, database=DB_NAME)
batch_size = 1000  # 적절한 배치 크기 설정
with connection.cursor() as cursor:
    for batch in batch_generator(batch_list, batch_size):
        formatted_ids = ', '.join(f'"{item}"' for item in batch)
        sql = f"""
        DELETE FROM table WHERE col1 IN ({formatted_ids})
        """
        cursor.execute(sql)
        connection.commit()
```