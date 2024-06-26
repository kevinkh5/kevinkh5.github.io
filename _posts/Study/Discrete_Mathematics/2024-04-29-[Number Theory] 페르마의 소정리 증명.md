---
title: "[Number Theory] 페르마의 소정리 증명"
author: baduk
date: 2024-04-29 11:59:00 +0900
categories: [Study, Discrete Mathematics]
tags: [Discrete Mathematics]
use_math: true
---
## 페르마의 소정리를 알아보기전 알아야 할 개념
페르마의 소정리를 알아보기전에, 먼저 알아야 할 몇 가지 용어와 개념들이 있다.

### 기약잉여계
기약잉여계란?
특정 수에 대해서 서로소인 정수의 집합을 뜻한다.

예를 들어 특정 수가 10일때, 10과 서로소인 정수는 1, 3, 7, 9이므로 {1, 3, 7, 9}가 10에 대한 기약잉여계가 된다.

주의할 것은 {1, 3, 7, 9}가 다있어야 그게 10의 기약잉여계가 될 수 있다. {1, 3} 이런식으로 일부만 포함된 것은 10의 기약잉여계가 될 수 없다.

### 소수의 기약잉여계
10과 같이 소수가 아닌수는 저렇게 나오지만, `소수의 기약잉여계는 신기하게도 1부터 자기자신-1 까지의 모든 요소의 집합이 된다.`

예를들어 소수 19의 기약잉여계는 {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18}가 된다.

이러한 경향은 소수가 1과 자기자신만을 약수를 갖는 특징 때문에 일어난다. 자기 자신보다 작고 1이상인 정수와의 공약수가 모두 1이기 때문에 즉, 서로소 이기 때문에 이러한 경향이 나타난다고 볼 수 있다.

### 합동식(Congruence)
- a ≡ b (mod n)

위 식을 해석하자면, a를 n으로 나눈 나머지와 b를 n으로 나눈 나머지가 같다는 뜻이다. 또는 b는 a를 n으로 나눈 나머지라고도 할 수 있다.(단, b가 0과 n-1사이의 정수일때만)

위 식을 읽을 때는 "a와 b는 법(=mod, modular) n에 대해서 합동"이라고 한다.

#### 합동식에서 양변에 같은 수를 더하거나 빼거나 곱하거나 하는 것은 자유롭다.
이게 무슨 말이냐면,

- 8 ≡ 13 (mod 5)

위 합동식이 있으면, 위 합동식 양변에 같은 수를 더하거나 빼거나 곱하거나 해서 아래처럼 만들어주어도 식이 성립한다는 것이다.

- 8+3 ≡ 13+3 (mod 5)
- 8-2 ≡ 13-2 (mod 5)
- 8* 4 ≡ 13*4 (mod 5)

나머지 연산은 순환적이라는 특징(예를들어 mod 5 면, 그 연산결과가 0부터~4까지의 범위만 내놓음)을 가지고 있다는 점을 이해하면, 양변에 뭘 더하든 같은 수를 더하기만 하면 같은 나머지가 나온다는 것은 위 예시를 통해 쉽게 받아 들일 수 있을 것이다. (빼기도 마찬가지로)

하지만 위 예시 마지막 합동식`8* 4 ≡ 13*4 (mod 5)`처럼, 합동식 양변에 같은 수를 곱했을 때도 식이 성립한다는 사실은 바로 이해하고 받아들이기가 어려울 것이다. 아래에서 합동식 양변에 같은 값을 곱했을 때도 식이 성립한다는 것을 증명해 보겠다.

예를들어 아래와 같은 합동식이 있다고 해보자.
- 3 ≡ 8 (mod 5)

위 합동식 양변에 M을 똑같이 곱했을 때, 아래와 같은 식이 될 것인데,
아래와 같은 식이 성립하는지 안하는지를 확인해야 한다.
- 3M ≡ 8M (mod 5)

우선 3과 8을 다르게 표현해보자.
- 3을 5로 나누었을 때의 몫과 나머지로 분리해서 표현하면 `3 = 5x + 3` 이 된다. (여기서 x=0일 것이다)
- 8을 5로 나누었을 때의 몫과 나머지로 분리해서 표현하면 `8 = 5y + 3` 이 된다. (여기서 y=1일 것이다)

3과 8을 몫과 나머지로 분리된 표현식 양변에 M을 곱하여 나타내면 아래와 같다.
- 3M = (5x + 3)M
- 8M = (5y + 3)M

그리고 위 두 식을 mod(법) 5에 대한 합동식으로 표현하면 아래와 같은 식이 만들어질 것이다.

- 3M ≡ (5x + 3)M ≡ 5xM + 3M (mod 5)
- 8M ≡ (5y + 3)M ≡ 5yM + 3M (mod 5)

위 두 식을 보면 알 수 있듯, 3M이나 8M 모두 5로 나눈 나머지가 3M으로 동일하다는 것을 알 수 있다. 즉, 수식으로 표현하자면 아래와 같다.
- 3M ≡ 8M (mod 5)

결론적으로 3 ≡ 8 (mod 5) 이 식에 양변에 M을 곱해도 합동식이 성립한다는것을 알 수 있다.

#### 합동식에서 양변에 같은 수를 더하거나 빼거나 곱하거나 하는 것은 자유롭다. BUT 나눗셈은 자유롭지 못하다!

합동식에서 덧셈,뺄셈,곱셈은 자유로우나, 나눗셈은 조심해야 한다.

예를들어 아래와 같은 합동식이 있다고 해보자. 

- Ca ≡  Cb (mod p)

위 합동식을 또 변형해서 아래 처럼 바꿔줄 수 있을 것이다.(합동식에서 양변에 같은 수를 더하거나 빼거나 곱하거나 하는 것은 자유로우니까. 그 이유는 위에서 설명하였다.)

- Ca-Cb ≡ 0 (mod p)

또 다르게 표현하자면, 아래 처럼 바꿔 줄 수 있다.

- C(a-b) ≡ 0 (mod p)

여기서 C(a-b)는 p로 나누었을 때 0이 된다라는 것을 알 수 있고, 따라서 C(a-b)는 반드시 p의 배수라는 것이라고 확정지을 수 있다. 여기서 문제! (a-b)는 p의 배수라고 할 수 있을까?

그건 아직 모른다. 만약 C가 p의 배수라면 (a-b)는 p의 배수일 수도 있고, 아닐 수도 있을 것이다.

그렇다면 C와 p가 서로소라면? 다시말해 C와 p의 공약수가 1 뿐이라면? 이 상황에서는 C(a-b)가 p의배수가 되기 위해서 반드시 (a-b)가 p의 배수가 되어야 할 것이다.

다시 정리해서 말하자면, C와 p가 서로소 이면, (a-b)가 `반드시 p의 배수`가 된다. 즉, 이걸 합동식으로 표현하자면, `(a-b) ≡ 0 (mod p)`가 성립한다.

위 식에서 b를 우변으로 보내주어 a ≡ b (mod p)  이와 같이 세울 수 있다.
(합동식에서 양변에 같은 수를 더하거나 빼거나 곱하거나 하는 것은 자유로우니까. 그 이유는 위에서 설명하였다.)

즉, 위 식은 Ca ≡ Cb (mod p)에서 C를 약분했을 때의 모습이라 할 수 있다. 결론적으로 C와 p가 서로소이기만 하면 C로 양변을 약분해도 식이 성립한다는 것을 알 수 있다.

주의할 점은 C와 p가 서로소가 아니어도 식이 성립할 수 는 있다. C와 p가 서로소가 아니어도 (a-b)가 p의 배수가 될 수는 있기 때문이다. 하지만 여기서 중요하게 기억해야 할 것은 `C와 p가 서로소이면 합동식에서의 양변을 C로 약분하는것이 무조건 가능`하다는 것이다.


### 배수 관계표현
- `a|b`

위 표현은 b는 a의 배수라는 뜻이다.

## 페르마의 소정리
페르마의 소정리가 무엇인지 알아보기전에, 먼저 아래 문제를 풀어봐라.


(문제) 7의 10제곱을 11로 나눈 나머지를 구하시오.
- $7^{10}$%11 = ? 

컴퓨터로 계산해서 풀면 물론 답이 뚝딱 나오겠지만, 직접 풀려고 하려면 7을 10번 곱하고 그걸 11로 나누면서 나머지를 구하는 고된 노동을 해야한다.

이때 페르마의 소정리를 안다면, 위 답은 1이라는 것을 바로 알 수 있다.

페르마의 소정리는 아래와 같다. 
- $a^{p-1}\equiv 1 \pmod{p} $

단, p가 소수이고, a와 p가 서로소여야만(= a와 p의 최대공약수가 1이면) 식이 성립한다.


이 페르마의 소정리 공식에 위 문제에서 주어진 수를 대입하면 아래와 같다
- $7^{11-1} \equiv 1 \pmod{11} $

따라서 페르마의 소정리를 활용하면 매우 큰 수에 대한 나머지 연산을 매우 빠르게 처리 할 수 있다. 또한 이를 활용하여 코딩할 때 쓰면, 오버플로우 방지와 효율적인 시간복잡도를 가진 코드작성이 가능해진다.

## 페르마의 소정리 증명
이제 왜 위 식이 성립하는지 증명해보겠다.
소수의 기약잉여계는 1부터 자기자신-1까지를 요소로 받아, 그 요소가 총 자기자신-1개라고 위에서 설명하였다.

예를 들어 소수 p의 기약잉여계는 S라고 하고, 그 S = {1, 2, 3, ... , p-1} 라고하자.

이때, p와 서로소인 a를 이 기약잉여계에 곱하면, {a, 2a, 3a, ..., (p-1)a} 가 될 텐데, 이를 s'이라고 하자.

여기서 우리의 첫 번째 목표는 s'도 p의 기약잉여계인지 아닌지를 증명하는 것이다.

먼저 스포를 하자면 s'은 p의 기약잉여계가 맞고, s가 가진 요소들의 곱과 s'이 가진 요소들의 곱을 p로 나눈 나머지가 같다는 점을 이용해서 페르마의 소정리 식을 도출해 낼 것이다.

## s'이 p의 기약잉여계인지 증명하기
아래와 같은 조건일때,
- 1 <= i < j <= p-1
- i,j 모두 정수

s'이 p의 기약잉여계려면, s'집합의 모든 요소에 대해 p로 나누었을때의 각 요소의 나머지가 서로 다 달라야 한다. 즉, (i * a)%p와 (j * a)%p가 동일한 것은 없어야 한다.

우리는 귀류법(특정 명제의 반대를 가정했을 때, 모순이 생기면, 그 명제가 참이라고 증명하는 수학적 증명 방법)을 통해 s'이 기약잉여계임을 증명할 것이기 때문에 (i * a)%p와 (j * a)%p가 같다고 가정할 것이다.

- (i * a)%p == (j * a)%p

위 식이 모순이 생긴다면, s'은 기양잉여계임이 증명될 것이다. 왜냐하면 s'집합 요소의 개수가 p-1개가 있는데, (i * a)%p != (j * a)%p라면, s'집합은 p의 기약잉여계임이 틀림없기 때문이다. s'의 요소의 개수는 p-1개로, s의 요소의 개수와 같은데, s'의 각요소를 p로 나눈 나머지가 모두 서로 다르다는 의미이기 때문이다.

즉, 어떠한 소수 p에 대해서 아래 조건을 모두 만족하면 기약잉여계인것이다.
- 요소의 개수가 p-1개다.
- 각 요소를 p로 나눈 나머지가 모두 다르다.

i* a ≡ j*a (mod p) 즉, `(i * a)%p = (j * a)%p`임을 가정했을 때, 

좌변`(i * a)%p`을 우변으로 넘겨,

(j * a)%p - (i * a)%p = 0

위 식을 세울 수 있다. 그리고 양변을 각각 p로 나누었을 때, 나머지도 같을 것이므로,

((j * a)%p - (i * a)%p)%p = (0)%p

을 계산해주면,

((j * a)%p - (i * a)%p)%p = 0

이 되고, 좌변은 모듈러 연산의 분배법칙으로 정리하면,

((j * a) - (i * a))%p = 0

위 식이되고 다시 좌변에 있는 a로 묶어주면, 아래와 같은 식이 된다.
- ((j-i)a)%p = 0

위 식이 성립하려면, 아래 4가지 조건중 하나라도 만족해야 한다.

- j = i
- `p | (j - i)` ( j-i가 p의 배수)
- a = 0
- `p | a` (a가 p의 배수)

결론적으로는 위 4가지 조건중 그 어느하나도 만족을 못하여, 위 식 `((j-i)a)%p = 0`이 성립하지 못하고, 위 식은 모순이 된다.

왜 만족못하는지 아래에서 하나씩 살펴보겠다.

### j = i ?
위 조건에서 i,j의 범위는 1 <= i < j <= p-1 라고 했다. 따라서 j와 i는 같을 수 없다. 땡.

### `p | (j - i)` ( j-i가 p의 배수)
일단 조건을 보면 알 수 있듯, j는 i 보다 큰 수이기 때문에 (j-i)는 일단 0보다 큰 정수다. 근데 j는 아무리 큰 숫자여봤자 최대 p-1이다. j를 최대로 늘려 p-1이라치고, i를 최소로 줄여 1이라 치더라도, (j-i)는 최대 p-2밖에 안된다.

즉 j-i는 p의 배수가 될 수 없다. 그 이유는 소수인 p보다 작은 수는 결코, p의 배수가 될 수 없기 때문이다. 예를들어 소수 p가 11이라고 했을 때, 11보다 작은 수 중 11의 배수는 절대 있을 수 없다. (소수는 그 자체로 아무것도 더해지지 않은 본래의 자신이란 점을 생각하면 쉽게 이해될 것이다.)

따라서 (j - i)는 p의 배수가 될 수 없다. 땡.

### a = 0
a는 소수 p와 서로소라고 가정했다. 따라서 0이 될 수도 없을 뿐더러, a가 0이면 위에서 제시한 s'= {a, 2a, 3a, ... ,(p-1)a}에서 s'의 모든 요소가 0이 되어버린다. 이렇게 되면 s' p의 기약잉여게라고 제시한 조건과도 맞지 않는다. 따라서 땡.

### `p | a` (a가 p의 배수)
a는 소수 p와 서로소라고 가정했다. 따라서 a는 결코 p의 배수가 될 수 없다. 따라서 땡.

### 결론:s'은 p의 기약잉여계다.
자 이렇게 위 4가지 조건 모두 만족할 수 없음을 보였고, `((j-i)a)%p = 0` 이 식이 틀렸다는 것을 증명하였다.

즉, 다시 말하자면, 아래와 같은 조건일때,
- 1 <= i < j <= p-1
- i,j 모두 정수

`(i * a)%p = (j * a)%p` 이 식은 성립하지 않는다. 다른말로 s'요소내에 수들을 p로 나누었을 때 그 나머지가 중복되는게 없다는 뜻으로 s'은 p의 기약잉여계란 뜻이다.

## 페르마의 소정리 식 도출하기
지금까지 s'이 p의 기약잉여계임을 증명하였다. 이제 이 점을 이용해서 페르마의 소정리 공식을 한번 도출해보자.

s(={1,2,3,...,p-1})와 s'(={a,2a,3a,...,(p-1)a}) 모두 p의 기약잉여계니까, 각 요소의 곱을 p로 나눈 나머지가 같을 것이므로, 아래 합동식은 성립할 것이다.

- $ (p-1)! ≡ a^{p-1}(p-1)! (mod p) $

자, 여기서 p는 소수이고 1,2,3,...,p-1과 서로소관계다. 즉 1,2,3,...,p-1로 양변을 약분할 수 있다. 따라서 양변을 (p-1)!로 약분하면 아래와 같은 합동식이 만들어질 것이다. (합동식의 나눗셈은 조심해야한다고 위에서 언급했듯, 반드시 약분할 수와 mod옆에 붙은 수가 서로소관계여야만 약분가능이 보장된다.)

- $ 1 ≡ a^{p-1} (mod p) $

일반적으로는 위 식 좌우변을 바꿔서 아래와 같이 표현한다.

- $ a^{p-1} ≡ 1 (mod p) $

위 식을 살짝 응용하자면, p와 서로소인 a로 양변을 또 나누어 주면 아래와 같은 합동식으로 또 표현할 수 있다.

- $ a^{p-2} ≡ a^{-1} (mod p) $


이렇게 페르마의 소정리 증명과 이를 이해하기 위해 알아야 할 개념들에 대해서 정리해보았다.