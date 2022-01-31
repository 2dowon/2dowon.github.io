---
layout: default
title: Operator
last_modified_date: 2021-02-08 16:02:77
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Operator</div>

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

Operator 연산자란 프로그래밍에서 쓰이는 기호들을 말한다.

## String concatenation 문자열 연산자

```jsx
console.log("my" + "cat"); // mycat
console.log("1" + 2); //12
console.log(`string literals: 1+2 = ${1 + 2}`); // string literals: 1+2 = 3
```

## Numeric operators 산술 연산자

```jsx
console.log(1 + 1); // add
console.log(1 - 1); // substract
console.log(1 / 1); // divide
console.log(1 * 1); // multiply
console.log(5 % 2); // remainder 나머지 값
console.log(2 ** 3); // exponentiation 2의 3승 =8
```

## Increment and decrement operators 증감 연산자

### preIncrement

: counter에 1을 더한 다음에 preIncrement에 할당한다는 뜻

```jsx
let counter = 2;
const preIncrement = ++counter;
console.log(`preIncrement: ${preIncrement}, counter: ${counter}`);
// preIncrement =3, counter=3
```

> preIncrement는 아래와 동일한 코드

```jsx
counter = counter + 1;
preIncrement = counter;
```

### postIncrement

: postIncrement에 먼저 counter값을 할당한 다음 1을 더해서 counter를 출력함

```jsx
let counter = 2;
const postIncrement = counter++;
console.log(`postIncrement: ${postIncrement}, counter: ${counter}`);
// postIncrement =2, counter=3
```

> postIncrement는 아래와 동일한 코드

```jsx
postIncrement = counter;
counter = counter + 1;
```

### preDecrement

: counter에 1을 뺀 다음에 preIncrement에 할당한다는 뜻

```jsx
let counter = 2;
const preDecrement = --counter;
console.log(`preDecrement: ${preDecrement}, counter: ${counter}`);
// preDecrement=1, counter=1
```

### postDecrement

: postDecrement에 먼저 counter값을 할당한 다음 1을 빼서 counter를 출력함

```jsx
let counter = 2;
const postDecrement = counter--;
console.log(`postDecrement: ${postDecrement}, counter: ${counter}`);
// postDecrement=2, counter=1
```

## Assignment operators 대입 연산자

반복되는 x를 생략해서 코드를 작성하는 방법

```jsx
let x = 3;
let y = 6;
x += y; // x = x + y;
x -= y; // x = x - y;
x *= y; // x = x * y;
x /= y; // x = x / y;
```

## Comparison operators 비교 연산자

비교하는 operators

```jsx
console.log(10 < 6); // less than
console.log(10 <= 6); // less than or equal
console.log(10 > 6); // greater than
console.log(10 >= 6); // greater than or equal
```

## Logical operators 논리 연산자

### || (or)

or 연산자는 처음으로 true 값이 나오면 멈춘다 (하나 이상이 ture이면 되기 때문)

그렇기 때문에 연산이 많은 함수를 제일 마지막에 두고, 심플한 value값을 앞에 둬야 한다

```jsx
const value1 = true;
const value2 = 4 < 2;

console.log(`or: ${value1 || value2 || check()}`);

function check() {
  for (let i = 0; i < 10; i++) {
    //wasting time => 시간을 낭비하다가 결국 true를 출력하는 함수
    console.log("🤪");
  }
  return true;
}

// or: true (value1이 true이기 때문에 true가 출력됨)
```

### && (and)

and 연산자는 처음으로 false값이 나오면 멈춘다 (모든 값이 true여야 하기 때문에)

간단하게 null을 체크할 때도 사용한다

> nullableObject가 null이 아닐때만 something을 가져올 수 있도록

```jsx
if (nullableObject != null) {
  nullableObject.something;
}
```

### ! (not)

값을 반대로 바꿔주는 역할

> value1이 true이기 때문에 false로 바꿔서 출력됨

```jsx
const value1 = true;
console.log(!value1); // false
```

## Equality operators 비교 연산자

### == (loose equlity)

type이 다른 경우에도 같다고 출력함

⇒ string 5와 number 5를 `==`으로 물어보면 true라고 출력

### === (strict equlity)

type이 다른 경우에 다르다고 출력함

⇒ string 5와 number 5를 `===`으로 물어보면 false라고 출력

> == 과 ===

```jsx
// object equlity by reference
const ellie1 = { name: "ellie" };
const ellie2 = { name: "ellie" };
const ellie3 = ellie1;
console.log(ellie1 == ellie2); // false 각각 다른 ref가 저장되어 있으므로
console.log(ellie1 === ellie2); // false
console.log(ellie1 === ellie3); // true

// equlity - puzzlere
console.log(0 == false); // true (emthy string은 다 false로 간주)
console.log(0 === false); // false (0은 boolean type이 아니므로)
console.log("" == false); // true (emthy string은 다 false로 간주)
console.log("" === false); // false (0은 boolean type이 아니므로)
console.log(null == undefined); // true
console.log(null === undefined); // false (type이 서로 다르므로)
```

## Conditional operators 조건 연산자 : if, else if, else

```jsx
const name = "dowon";
if (name === "dowon") {
  console.log("Welcome, Dowon!");
} else if (name === "coder") {
  console.log("You are amazing coder");
} else {
  console.log("unkwon");
}
// Welcome, Dowon!
```

## Ternary operators 삼항 연산자

`condition ? value1 : value2;` ⇒ `조건 ? 참:거짓`

⇒ condition인 ?가 true면 value1을 출력하고, false이면 value2를 출력한다

가독성을 위해서 간단한 코드일때만 사용하는 것이 좋다

```jsx
const name = "dowon";
console.log(name === "dowon" ? "yes" : "no");
// yes
```

## Switch operators

if, if else를 계속 반복한다면 switch 사용을 고려하는 것이 좋다

```jsx
const browser = "IE";
switch (browser) {
  case "IE":
    console.log("go away!");
    break;
  case "Chrome":
  case "Firefox":
    console.log("love you!");
    break;
  default:
    console.log("same all!");
    break;
}
```

# Ref.

- [자바스크립트 4. 코딩의 기본 operator, if, for loop 코드리뷰 팁 - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=YBjufjBaxHo&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=4)

- [표현식과 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators)
