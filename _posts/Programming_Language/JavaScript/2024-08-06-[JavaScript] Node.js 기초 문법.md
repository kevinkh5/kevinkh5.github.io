---
title: '[JavaScript] Node.js 기초 문법'
author: baduk
date: 2024-08-17 14:59:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---

## 입력받은 값 그대로 출력하기
```javascript
const readline = require('readline');
//  Node.js에서 기본적으로 제공되는 readline 모듈을 불러오기
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
    // input: process.stdin: 표준 입력 스트림(터미널 입력)을 사용합니다.
    // output: process.stdout: 표준 출력 스트림(터미널 출력)을 사용합니다.
});
// readline.createInterface 메서드를 사용하여
//  입력과 출력을 처리하는 rl 인터페이스 만들기

let input = [];

// on(eventName, callback) 형태로 사용됩니다. 
// 여기서 eventName은 감지하고자 하는 이벤트의 이름,
//  callback은 해당 이벤트가 발생했을 때 호출될 함수
rl.on('line', function (line) {
    // Enter 키를 누르면 이 이벤트가 발생
    input = [line];
}).on('close', function () {
    // readline 인터페이스가 닫힐 때 발생하는 이벤트
    // ctrl + D 하면 발생
    str = input[0];
    console.log(str)
});
```