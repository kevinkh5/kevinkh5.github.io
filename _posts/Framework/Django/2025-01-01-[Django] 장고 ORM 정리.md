---
title: "[Django] 장고 ORM 정리"
author: baduk
date: 2025-01-01 09:14:00 +0900
categories: [Framework, Django]
tags: [Django]
---
## Django ORM
- 장고에서 테이블은 모델, 레코드는 인스턴스.
- 장고은 모델클래스 단위로 쿼리함.
- ORM은 쿼리셋이라는 iterable한 객체를 통해 데이터를 반환함.
    - ex) queryset = User.objects.all()

## 쿼리 확인
- 쿼리셋의 쿼리는 str(queryset.query)로 확인가능
- sqlparse 사용하면 sql 보기 좋게 출력가능

```python
import sqlparse

query = str(queryset.query)
print(sqlparse.format(query, reindent=True))
```

## 쿼리 성능 분석
- sql Execution Plan
```python
print(queryset.explain())
```

## 전체 조회
```python
User.objects.all()
```
```sql
SELECT *
FROM User
```

## 조건 걸기
```python
User.objects.filter(is_active=True)
```
```sql
SELECT *
FROM User
WHERE is_active = 1
```

## 집계 함수 (Count)
```python
User.objects.aggregate(Count('id'))
```
- 위와 같이 Alias 설정하지 않으면 아래와 같은 이름이 디폴트로 지정됨.
```sql
SELECT COUNT(id) AS id_count
FROM User
```

```python
User.objects.aggregate(count=Count('id'))
```
- 위와 같이 alias추가 가능

**아래와 같이 장고에서 제공하는 메소드로 count가능**
```python
User.objects.count()
```
```sql
SELECT COUNT(*)
FROM User
```
**주의 할 것은 집계할 때 NULL값은 아예 연산에서 제외된다는것 잊지말기** 

## 날짜 조건
```python
User.objects.filter(
    joined_at__gte=date(2022,1,1),
    joined_at__lte=date(2022,12,31)
)
```
```sql
SELECT *
FROM User
WHERE joined_at >= '2022-01-01' AND joined_at <= '2022-12-31'
```

## 날짜 조건(range)
```python
User.objects.filter(
    joined_at__range=(
        date(2022,1,1), date(2022,12,31)
    )
)
```
```sql
SELECT *
FROM User
WHERE joined_at BETWEEN '2022-01-01' AND '2022-12-31'
```

## 날짜 조건(DateTimeField)
```python
User.objects.filter(
    joined_at__range=(
        datetime(2022, 1, 1), datetime(2022, 12, 31)
    )
)
```

```sql
SELECT *
FROM User
WHERE joined_at >= '2022-01-01 00:00:00' AND joined_at <= '2022-12-31 00:00:00'
```

## 날짜 조건(권장사항 exclusive하게 처리)
```python
User.objects.filter(
    joined_at__gte=date(2022, 1, 1),
    joined_at__lt=date(2023, 1, 1)
)
```

```sql
SELECT *
FROM User
WHERE joined_at >= '2022-01-01 00:00:00' AND joined_at <= '2023-01-01 00:00:00'
```

## 유니온
```python
books = Book.objects.all()
ebooks = Ebook.objects.all()

books.union(ebooks)
```

```sql
(SELECT *
FROM Book)
UNION (
SELECT *
FROM EBook))
```

## 유니온(유니크 검사X)
```python
books.union(ebooks, all=True)
```
```sql
(SELECT *
FROM Book) UNION ALL (
SELECT *
FROM EBook))
```

## 새로운 컬럼값 추가(Annotate)
```python
from django.db.models import Value

books = Book.objects.annotate(
    book_type=Value('일반책')
)
ebooks = Ebook.objects.annotate(
    book_type=Value('e-book)
)

books.union(ebooks, all=True)
```

```sql
(
SELECT *
FROM Book ) UNION ALL (
SELECT *
FROM EBook
)
```

## 컬럼간 연산(F)
```python
from django.db.models import F

Book.objects.annotate(
    sale_price=F('price') - F('discount')
)
```

```sql
SELECT id, title, price, discount, price - discount AS sale_price
FROM Book
```

## 특정 컬럼의 결과만 받기(values, values_list)
```python
User.objects.values('id', 'name')
```
- 이렇게 하면 아래와 같이 딕셔너리 형태로 반환
```
출력
<QuerySet [{'id': 1, 'name': 'Guido'}]>
```

```python
User.objects.values_list('id', 'name')
```
- 이렇게하면 아래와 같이 튜플형태로 값만 반환
```
출력
<QuerySet [(1, 'Guido')]>
```
```sql
SELECT id, name
FROM User
```

## 레프트 아우터 조인
```python
User.objects.select_related('profile')
```

```sql
SELECT *
FROM User U
LEFT OUTER JOIN Profile P ON U.id = P.user_id
```

**아래와 같이 values를 사용하면 조인이 무시됨**
```python
User.objects.select_related('profile').values()
```
```sql
SELECT *
FROM User
```

## 특정 컬럼만 select하고 싶을때는 only
```python
User.objects.only('name')
```
- 위 장고ORM코드는 아래와 같은 결과가 나온다. id는 항상 포함된다.
```sql
SELECT id, name
FROM User
```

## annotate와 F사용해서 새로 연산한 값을 칼럼에 추가한 예시
```python
Book.objects.annotate(sale_price=F('price') - F('discount')
)
.values(
    _sale_price=F('sale_price'),
    _price=F('price')
    _discount=F('discount'),
)
```
```sql
SELECT price - discount AS _sale_price,
price AS _price,
discount AS _discount
FROM Book
```
- 언더스코어를 넣은 이유는 원래 칼럼이랑 같은 칼럼으로 alias를 지정할 수 없어서임.


## group by로 집계하기1
```python
User.objects.values('type').annotate(count=Count('id'))
```
```
출력
<Queryset [{'type':'A','count':2}, {'type':'B', 'count':1}]>
```

```sql
SELECT type, COUNT(id) AS count,
FROM User
GROUP BY type
```

- 만약 위 장고 orm코드에서 **.annotate(count=Count('id'))** 이 부분을 제거하고 출력하면 아래와 같다.
- values는 아래와 같이 특정 컬럼을 기준으로 딕셔너리 형태로 쿼리셋을 생성한다.
```python
User.objects.values('type')
```
```
출력
<Queryset [{'type':'A'}, {'type':'A'}, {'type':'B'}]>
```

## group by로 집계하기2
```python
User.objects.values('type','age').annotate(count=Count('id'))
```

```sql
SELECT type,age,COUNT(id) AS count,
FROM User
GROUP BY type, age
```

## like
```python
User.objects.filter(name__contains='Guido')
```
```sql
SELECT *
FROM User
WHERE name LIKE '%Guido%'
```
- 위와 같이 하면 Guido앞에 무언가 쓰여져 있는 이름까지 찾으므로 인덱스가 무시됨
- 따라서 아래와 같이 startswith= 'Guido'로 하면 인덱스 활용이 가능해짐

```python
User.objects.filter(name__startswith='Guido')
```
```sql
SELECT *
FROM User
WHERE name LIKE 'Guido%'
```

## 날짜 
```python
User.objects.filter(joined_at__year=2022)
```
```sql
SELECT *
FROM USER
WHERE YEAR(joined_at) = '2022'
```
- 위와 같이 작성하면 index활용이 안되므로 아래와 같이 날짜를 조회한다.
```python
User.objects.filter(joined_at__gte='2022-01-01', joined_at__lt='2023-01-01')
```
```sql
SELECT *
FROM USER
WHERE joined_at >= '2022-01-01' AND joined_at < '2023-01-01'
```

## OR 조건
```python
USER.objects.filter(Q(id=1)| Q(type='A'))
```
```sql
SELECT *
FROM USER
WHERE id = 1 OR type = 'A'
```
- 위와 같이 작성하는 것보다 아래와 같이 작성하는것이 더 빠를 수 있음
- 그 이유는 두개의 쿼리가 각각 Index를 사용하여 조회된뒤 병합되기 때문임

```python
USER.objects.filter(id=1).union(User.objects.filter(type='A'))
```
```sql
SELECT *
FROM USER
WHERE id = 1 ) UNION (
SELECT *
FROM USER
WHERE type = 'A')
```

## case문
```python
cities = (
    City.objects.annotate(
        search_order = Case(
            When(name='Seoul', then=Value(1)),
            default=Value(0),
            output_field=IntegerField(),
        )
    ).order_by('-search_order')
)
```
```sql
SELECT *,CASE WHEN name = 'Seoul' THEN 1 ELSE 0 END AS search_order
FROM City
ORDER BY search_order DESC
```



