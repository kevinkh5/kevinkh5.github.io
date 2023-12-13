---
title: '[Javascript] Type Coercion'
author: baduk
date: 2023-12-13 10:54:00 +0900
categories: [Study, Javascript]
tags: [study]
---
[Javascript] Type Coercion?
```
Type Coercion refers to the process of automatic or implicit conversion of values from one data type to another.
```
자바스크립트는 파이썬이랑 다르게 형변환을 하지않더라도 자동으로 형변환이 되는 기능이 있다.

```javascript
let a = '5'
let b = 3
console.log(typeof a) //string
console.log(typeof b) //number
console.log(typeof (a+b)) // string
console.log(typeof (a-b)) // number
console.log(a+b) // 53
console.log(a-b) // 2
```
위와같이 스크립트에서도 볼 수 있듯, 더했을 때는 number타입이 string타입으로 자동변환되고, 이상하게 뺄 때는 string타입이 number타입으로 자동변환된다.

자바스크립트를 이제 막 배우기 시작한 사람한테는 혼란스러울 수 있을 것 같다.😂