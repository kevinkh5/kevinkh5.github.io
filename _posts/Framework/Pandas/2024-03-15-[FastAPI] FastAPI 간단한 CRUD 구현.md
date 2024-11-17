---
title: "[Pandas] 판다스 자주 사용하는 코드"
author: baduk
date: 2024-11-17 15:38:00 +0900
categories: [Framework, Pandas]
tags: [Pandas]
---
## **파일관련 (load & save)**

### 엑셀 파일 읽기
```python
import pandas as pd
# 엑셀 파일 읽기
file_path = '엑셀파일.xlsx'  # 읽어올 파일 경로
df = pd.read_excel(file_path, sheet_name='sheet1')  # 특정 시트 읽기 (생략하면 첫 번째 시트)
```

### CSV save & load
```python
import pandas as pd
#저장 
df.to_csv('csv파일.csv', index=False, encoding='utf-8-sig')
# CSV 파일 로드
df = pd.read_csv('csv파일.csv')
# CSV 파일 로드하다 에러나면 아래꺼로!
df = pd.read_csv('test_error.csv', lineterminator='\n')
```

## **데이터 타입 변환**

### df(csv) -> json 변환
```python
json_result = df.to_json(orient='records', force_ascii=False)
# orient='records'로 해줘야 [{"col1":1,"col2":2},{"col1":3,"col2":4}]
# 의도한대로 json형태의 데이터로 변환 가능
# force_ascii를 False로 둬야 비ASCII 문자를 그대로 사용하여, 가독성 높일 수 있음
json_result = json.loads(json_result)
```

### json -> csv 변환
```python
df = pd.DataFrame(json_variable)
```

## **전처리 관련**

### row 이터레이션
```python
import pandas as pd

# 예시 데이터프레임 생성
data = {
    'video_id': [1, 2, 3],
    'title': ['Video A', 'Video B', 'Video C'],
    'views': [100, 200, 300]
}
df = pd.DataFrame(data)

# DataFrame의 각 행을 반복
for index, row in df.iterrows():
    # 각 행의 데이터에 접근
    print(f"Index: {index}, Video ID: {row['video_id']}, Title: {row['title']}, Views: {row['views']}")
```

### row 추가
```python
columns = ['video_id', 'title', 'result']
result_df = pd.DataFrame(columns=columns) # 데이터프레임 만들어서
new_row = pd.DataFrame([{'video_id':video_id, 'title': title, 'result': result}]) # 추가할 row를 만들어서
result_df = pd.concat([result_df, new_row], ignore_index=True) # 합치기
```

### row 수정
```python
for index, row in df.iterrows():
    df.at[index, 'new_col'] = 'new_val'
```


### 중복된 row를 지우고 싶을 때
```python
df = df.drop_duplicates()
# 
```

### 중복된 column을 지우고 싶을 때
```python
df = df_filtered.T.drop_duplicates().T
# .T는 데이터프레임의 전치(transpose)를 의미
# 
```

### 데이터 프레임 붙이기
```python
df_combined = pd.concat([df1, df2], axis=1)
# axis를 1로 주면 좌우로 붙이고, 0으로 주면 위 아래로 붙임
```

### 데이터 프레임 칼럼 이름 바꾸기
```python
# 칼럼 이름 변경
df.rename(columns={'id': 'new_id', 'title': 'new_title'}, inplace=True)
```

### 특정 칼럼만 남기기
```python
# 남기고 싶은 칼럼 목록
columns_to_keep = ['video_id', 'title']
# 특정 칼럼만 남기고 나머지 제거
df_filtered = df[columns_to_keep]
```

### 특정 칼럼만 지우기
```python
# 특정 칼럼 제거
df_filtered = df_filtered.drop(columns=['col1','col2'])
```

## **데이터 체크 관련**

### nan 체크
```python
if pd.isna(value):
    value = "" # NaN 이면 ""으로 바꾸는 코드
```

### 특정 조건에 맞는 데이터 보기
```python
filtered_rows = df[(df['title'] == '') & (df['description'].notna())]
# title이 ''이고, description이 Nan이 아닌것의 개수
```

### 특정 조건에 따른 개수 구하기
```python
count_not_empty_strings = df[df['title'] != ''].shape[0] # ''값이 아닌것의 개수
not_nan_count = df['title'].notna().sum() # NaN이 아닌것의 개수
nan_count = df['title'].isna().sum()
```

### 특정 키로 접근 할 수 있는 딕셔너리로 바꾸기
```python
unique_dict = {row['video_id']: row for index, row in df.iterrows()}

unique_dict_for_product_list = {row['video_id']: row for index, row in df.iterrows() if row['video_id'] in video_id_list} # 조건 걸기
```
