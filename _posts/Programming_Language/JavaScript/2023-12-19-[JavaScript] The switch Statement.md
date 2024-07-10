---
title: '[JavaScript] switch문법에서 break사용'
author: baduk
date: 2023-12-19 12:02:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---
### [Javascript] The switch Statement
```javascript
const a = 1
switch(a){
    case 1:
        console.log('Your input is 1')
    case 2:
        console.log('Your input is 2')
}
```

위 코드를 실행하면 프린트 결과로 어떻게 출력될 것 같은가?

`Your input is 1`

위와 같이 출력될거라고 생각할 수 있지만, 결과는 아래와 같이

`Your input is 1`

`Your input is 2`

두 프린트문이 출력되게 된다. 따라서 switch를 사용할 때는 아래 코드와 같이 break 사용을 고려해야 한다.

```javascript
const a = 1
switch(a){
    case 1:
        console.log('Your input is 1')
        break // break 추가
    case 2:
        console.log('Your input is 2')
}
```
break를 해주어야 아래 코드가 실행되지 않고 해당 포인트에서 멈출 수 있다.