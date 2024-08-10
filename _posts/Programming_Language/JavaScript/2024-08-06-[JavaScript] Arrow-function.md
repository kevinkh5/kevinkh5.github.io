---
title: '[JavaScript] Arrow-function'
author: baduk
date: 2024-08-06 14:18:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---

## 일반적인 함수 표현
```javascript
function simple_fun() {
    console.log('baduk');
}
simple_fun() // baduk
```

## 자바스크립트의 Arrow-function


### console.log 리턴
```javascript
const simple_arrow_fun1 = () => {
    console.log('baduk');
}
simple_arrow_fun1() // baduk
```


### number 입력 & 리턴
```javascript
const simple_arrow_fun2 = (x, y) => { return x + y }
console.log(simple_arrow_fun2(2, 3)) // 5
```

```javascript
const simple_arrow_fun2 = (x, y) => x + y
console.log(simple_arrow_fun2(2, 3)) // 5
```