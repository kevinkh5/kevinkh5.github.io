---
title: '[JavaScript] 프린트문 안에 변수 출력하기'
author: baduk
date: 2023-12-19 12:25:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---
### [Javascript] Statements and Expressions
```javascript
const type_number = 1 + 3
console.log(typeof type_number)
const type_str = 'ABC'
console.log(`type_number: ${type_number}, type_str: ${type_str}`)
```

위 코드를 실행하면 아래와 같이 출력 된다.

```
number
type_number: 4, type_str: ABC
```
이 처럼 변수의 값을 그대로 사용하여 프린트 출력을 원한다면, 백틱(``)을 사용한 후 사용할 변수를 중괄호 씌워주고 앞에 달러($)를 붙여주어 사용하면 된다. number타입이 되었든 str타입이 되었든 모두 사용가능하다.