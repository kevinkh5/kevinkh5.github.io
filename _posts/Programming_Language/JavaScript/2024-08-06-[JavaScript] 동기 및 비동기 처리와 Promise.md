---
title: '[JavaScript] 동기 및 비동기 처리와 Promise'
author: baduk
date: 2024-08-06 14:36:00 +0900
categories: [Programming Language, JavaScript]
tags: [study]
---

## 자바스크립트의 비동기 처리
자바스크립트는 REST API 요청, 파일/DB 처리, 타이머, 암호화/복호화에 대해서는 비동기 처리를 한다.

### 타이머 비동기 처리 간단 예시
```javascript
// setTimeout 함수의 기본 형태
// setTimeout(function, delay);

console.log("I like coding!");
setTimeout(() => {
    console.log('I can do it!')
}, 3000); // 3000ms -> 3초
console.log("Kevin")

// I like coding!
// Kevin
// I can do it!
```

만약 동기적으로 처리될경우 3초를 기다리고 Kevin을 출력을 해야 하는데, 비동기적으로 처리하므로 Kevin을 먼저 출력하고, 이후 I can do it!을 출력한다.


## 만약 비동기적으로 출력하지 않고, 동기적으로 출력하고 싶다면?

아래와 같이 자바스크림트의 first-class function의 특징을 이용해서 동기적으로 처리할 수 있다.

```javascript
console.log("I like coding!");
function desc(callback) {
    setTimeout(() => {
        console.log('Kevin');
        callback();
    }, 3000);
}
function desc2() {
    console.log('I can do it!');
}

desc(desc2);

// I like coding!
// Kevin
// I can do it!
```
함수를만들어 함수의 인자로 다음에 실행할 함수를 넣어 실행한다. 그리고 비동기적으로 실행될 수 있는 함수안에 인자로 받은 함수를 넣어주면, 동기적으로 실행한다.


## 콜백지옥
하지만 여러개의 코드를 동기적으로 실행되도록 처리한다면 아래와 같이 길고 가독성 떨어지는 코드를 작성해야 할 것이다.

```javascript
function func1(callback) {
    setTimeout(() => {
        console.log("1");
        setTimeout(() => {
            console.log("2");
            setTimeout(() => {
                callback();
            }, 500);
        }, 500);
    }, 500);
}

function func2() {
    console.log("3");
}
func1(func2);
// 1
// 2
// 3
```

## Promise 문법
위 콜백지옥과 같은 문제를 극복하기 위해 Promise를 사용해야 한다.

- Promise 객체 안에서는 executor라는 함수가 내부적으로 실행된다.
- executor라는 함수는 resolve와 reject라는 두 개의 함수를 인자로 받아서 비동기 처리 함수를 실행한다.
- 비동기 처리함수의 실행이 끝나고, 작업이 성공하면 resolve, 실패하면 reject함수를 호출한다.
- Promise는 Pending(대기), Fulfilled(이행), Regjected(실패)와 같은 3가지 상태가 있다.


```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 10;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
}, (err) => {
    console.log(err);
})
console.log('baduk1');

// baduk1
// success 10
```

## then() 메서드
then() 메서드를 이용하면, 해당 코드 작업 성공 또는 실패시 수행할 작업을 정의하여 실행할 수 있다.

- promise.then(successCallback, failureCallback)
- promise.then(successCallback)

```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 10;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
}, (err) => {
    console.log(err);
}).then(() => {
    console.log('baduk2');
}, () => {
    console.log('err2')
})

// success 10
// baduk2
```
위 코드는 resolve로 작업 성공했으니, "err2"가 아닌 "baduk2"를 출력하였다.

## 체이닝

then을 활용하여 아래와 같이 비동기 작업을 순차적으로 처리할 수 있다.
```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 10;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
}, (err) => {
    console.log(err);
}).then(() => {
    console.log('baduk2');
}, () => {
    console.log('err2');
}).then(() => {
    console.log('baduk3');
}, () => {
    console.log('err3');
})

// success 10
// baduk2
// baduk3
```

아래와 같이 return을 통해 다음 then으로 인자도 전달할 수 있다. return된 값은 아래 코드에서 data변수로 전달된다.
```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 10;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
    return 30
}, (err) => {
    console.log(err);
}).then((data) => {
    console.log(data);
    console.log('baduk2');
}, () => {
    console.log('err2');
})

// success 10
// 30
// baduk2
```


## catch() 메소드

catch() 메소드는 예외처리하는 메소드다. 아래는 Promise객체안에서 reject됐을 때 catch()메소드가 호출되는 예시코드다.
```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 8;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.catch((error) => {
    console.log(error);
});
// error
```

만약 아래와 같이 then() 메소드에서 실패했을 때 실행되는 함수가 실행되면, catch()메소드는 호출되지 않는다. 
```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 8;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
}, () => {
    console.log('error 발생');
}).catch((error) => {
    console.log(error);
});
//error 발생
```

하지만 아래와 같이 then() 메소드에서 failureCallback이 정의 되어있지 않으면, catch() 메소드는 호출된다.
```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 8;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);


runCode.then((item) => {
    console.log('success', item);
}).catch((error) => {
    console.log(error);
});
//error
```

## throw
throw는 사용자 정의 예외를 줄 때 사용한다.

```javascript
const runCode = new Promise(
    (resolve, reject) => {
        throw new Error('에러가 발생함')
        setTimeout(() => {
            let num = 8;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);


runCode.then((item) => {
    console.log('success', item);
}).catch((error) => {
    console.log('in catch')
    console.log(error);
});
// in catch
// Error: 에러가 발생함
```

## finally() 메소드
finally()메소드는 성공했든 못했든 무조건 실행하라는 의미의 메소드다.

```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 10;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
}).finally(() => {
    console.log('finally')
})
// success 10
// finally
```

중간에 예외가 발생하더라도 finally가 먼저 있어서, catch보다 finally가 먼저 실행되고 이후 예외처리에 대한 실행으로 catch가 실행된다.
```javascript
const runCode = new Promise(
    (resolve, reject) => {
        setTimeout(() => {
            let num = 10;
            if (num > 9) {
                resolve(num);
            } else {
                reject("error");
            }
        }, 1000);
    }
);

runCode.then((item) => {
    console.log('success', item);
    throw new Error("에러발생")
}).finally(() => {
    console.log('finally')
}).catch((err) => {
    console.log("catch error")
    console.log(err)
})
// success 10
// finally
// catch error
// Error: 에러발생
```

## Promise.all
- 동기적으로 처리할 Promise를 묶어서(묶여진 요소간에는 비동기적으로 처리됨) 한번에 실행하기 위해 사용한다.
- 묶여진 함수가 다 실행이되고나서, then 구문을 실행한다.

```javascript
const fun1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('a')
        resolve("a");
    }, 200);
});

const fun2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('b');
        resolve("b");
    }, 100);
});

const fun3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('c');
        resolve("c");
    }, 300);
});
Promise.all([fun1, fun2, fun3]).then((data) => {
    console.log(data);
})
// b
// a
// c
// ['100ms', '200ms', '300ms']
```

## Promise.race

race는 묶여진 함수들 중 제일 빠르게 끝난 하나의 함수의 리턴값을 받아서 then 메소드로 넘겨주어 처리한다. 그리고 이어서 나머지 함수들은 순차적으로 실행한다. 단, 이때는 나머지 함수의 리턴 값을 then으로 넘겨주지는 않는다.

```javascript
const fun1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('a')
        resolve("a");
    }, 200);
});

const fun2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('b');
        resolve("b");
    }, 100);
});

const fun3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('c');
        resolve("c");
    }, 300);
});
Promise.race([fun1, fun2, fun3]).then((data) => {
    console.log(data);
})
// b
// b
// a
// c
```