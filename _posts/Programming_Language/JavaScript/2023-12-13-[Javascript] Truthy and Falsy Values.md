---
title: '[JavaScript] Truthy and Falsy Values'
author: baduk
date: 2023-12-13 12:03:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---
### [Javascript] Truthy and Falsy Values
```javascript
console.log(Boolean(0)) // false
console.log(Boolean(undefined)) // false
console.log(Boolean('kevin')) // true
console.log(Boolean({})) // true
console.log(Boolean('')) // false
```

`{  }` 비어있는 중괄호는 `true`,

`'  '` 비어있는 Single quote는 `false`가 출력됨