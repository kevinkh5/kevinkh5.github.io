---
title: "[FastAPI] FastAPI 간단한 CRUD 구현"
author: baduk
date: 2024-03-15 17:06:00 +0900
categories: [Skill, FastAPI]
tags: [Fast API]
---
## Fast API

-Starlette 웹 프레임워크 & Pydantic(데이터 검증라이브러리) 기반으로 매우 빠른 성능

-타입 힌트를 활용한 유효성 검사로 디버깅 시간 절약

-Swagger UI 활용한 쉬운 API 테스트

-async await사용한 비동기 프로그래밍 지원

-확장성 좋음

-복잡한 기능과 인증 시스템 빌드할 때 유용한 종속성 주입시스템 제공

## RESTful API
Api 소프트웨어나 어플리케이션간 상호작용을 가능하게하는 도구와 규약(프로토콜)의 집합.

## Rest Architecture

균일한 인터페이스
- REST API는 자원(데이터)에 대해 일관된 인터페이스를 제공해야 한다

무상태
- 서버는 클라이언트의 상태를 관리하지 않는다. 즉 이전 요청과 상관없이 각 요청을 독립적으로 처리한다.

계층화 시스템
- 계층화 시스템은 시스템을 여러 계층으로 나누고, 각 계층은 서로 독립적으로 작동하며 통신할 수 있는 구조를 뜻한다.

캐시 가능성
- 캐싱을 통해 응답을 저장하고 재사용할 수 있어야 한다. 이는 네트워크 사용량을 줄이고 응답 시간을 개선하는 데 도움이 된다.

온디맨드 코드
- 클라이언트에게 필요한 코드나 스크립트를 제공하지 않고, 클라이언트가 필요한 리소스에 대해 요청을 보내도록 한다.

### Restful API는 CRUD작업을 웹을통해 수행할 수 있게 도와주는 수단
Create - POST

Read - GET

UPDATE - PUT/PATCH

DELETE - DELETE


## FastAPI 간단한 CRUD 구현
```python
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel # 유효성 검사를 수행하고 타입변환 자동처리

app = FastAPI()

@app.get("/helloworld")
def get_helloworld():
    return "hello world"

items = {
    0:{"name": "bread",
       "price":1000},
    1:{"name":"water",
       "price":500},
    2:{"name":"라면",
       "price":1200}
}

# Path parameter
@app.get("/items/{item_id}")
def read_item(item_id: int):

    if item_id not in items:
        return {"error": f"there is no item id:{item_id}"}
    item = items[item_id]
    return item

@app.get("/items/{item_id}/{key}")
def read_item_and_key(item_id: int, key:str):
    item = items[item_id][key]
    return item

# Query parameter
# ex) http://127.0.0.1:8000/item-by-name?name=bread
@app.get("/item-by-name")
def read_item_by_name(name: str):
    for item_id, item in items.items(): # key, value 같이 보내줌
        if item['name'] == name:
            return item
    return {"error": "data not found"}

class Item(BaseModel):
    name: str
    price: int

# 여기는 스웨거를 통해 테스트 해야 함
@app.post("/items/{item_id}")
def create_item(item_id: int, item: Item):
    if item_id in items:
        return {"error": "there is already existing key."}
    #여기서 넘어온 item의 그 형태 BaseModel을 상속받는 이 Item클래스이고
    #아이템이 딕셔너리로 관리되고 있기 때문에 동일타입유지를 위해 dict로 바꿔서 넣어줌
    #BaseModel을 통해 유효성 검사를 하는 이유는 name, price가 모두 있어야만 통과되도록 하기위함
    #하나라도 없으면 통과못하고,다른 이상한 키를 보내면, 걸러져서 옴
    items[item_id] = dict(item)
    print(items)
    return {"success":"ok"}

class ItemForUpdate(BaseModel):
    name: Optional[str]
    price: Optional[int]
    # 둘중 하나만 와도 괜찮기 때문에 Optional 붙임

@app.put("/items/{item_id}")
def update_item(item_id: int, item: ItemForUpdate):
    if item_id not in items:
        return {"error":f"there is no item id: {item_id}"}
    if item.name:
        items[item_id]['name'] = item.name
    if item.price:
        items[item_id]['price'] = item.price
    return {"success": "ok"}

@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    items.pop(item_id)
    return {"success": "ok"}
```
