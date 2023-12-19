---
title: '[JavaScript] 조건문을 간단하게 작성하는 또 다른 방법'
author: baduk
date: 2023-12-19 12:25:00 +0900
categories: [Study, JavaScript]
tags: [study]
---
### [Javascript] Ternary Operator
```javascript
const age = 15
age >= 18 ? console.log('I like to drink wine🍷'): // age가 18이상이면 출력
console.log('I like to drink water🧊'); // 그 외에는 여기를 출력
```
자바스크립트에서는 위 코드와 같이 if문을 한줄짜리 코드로 작성이 가능하다.

---
```javascript
const age = 15
const drink = age >= 18 ? 'wine🍷': 'water🧊';
console.log(drink) // water🧊
```
또한 위와 같이 조건에 맞으면 wine을 drink변수에 넣어주고, 조건이 맞지 않으면 water를 drink변수에 넣어서 선언하여 표현하는 방법도 있다. 

---
```javascript
const age = 15
console.log(`I like to drink ${age >= 18 ? 'wine🍷': 'water🧊'}`) // I like to drink water🧊
```
위와 같이 ${ } 괄호 안에 조건을 넣어서 출력하는 방법도 있다.
