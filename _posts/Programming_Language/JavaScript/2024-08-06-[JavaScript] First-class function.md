---
title: '[JavaScript] First-class function'
author: baduk
date: 2024-08-06 14:03:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---
## 자바스크립트의 First-class function 특징

### 1. 함수의 리턴 값 출력 가능
```javascript
const sayHello = function() {
    return "Hello, world!";
};

console.log(sayHello()); // 출력: "Hello, world!"
```

### 2. 특정 함수에 또 다른 함수를 인자로서 전달 가능
```javascript
function greet(greetingFunction) {
    console.log(greetingFunction());
}
greet(sayHello); // 출력: "Hello, world!"
```

### 3. 함수가 다른 함수를 반환하고, 반환된 함수를 특정 변수에 넣어서 실행 가능
```javascript
function createGreeter(name) {
    return function() {
        return `Hello, ${name}!`;
    };
}
const greetJohn = createGreeter('John');
console.log(greetJohn()); // 출력: "Hello, John!"
```

### 4. 배열에 함수를 저장하고, 꺼내쓰는 것 가능
```javascript
const functionsArray = [
    function() { return "First function"; },
    function() { return "Second function"; }
];

console.log(functionsArray[0]()); // 출력: "First function"
console.log(functionsArray[1]()); // 출력: "Second function"
```