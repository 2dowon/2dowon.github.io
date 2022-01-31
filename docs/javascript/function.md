---
layout: default
title: Function
last_modified_date: 2021-02-09 11:02:27
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Function</div>

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

- 일반적으로 함수는 sub program 하위 프로그램이다.
- 값은 함수에 전달될 수 있고 함수는 그 값을 반환한다.

  ![function](/assets/images/javascript/function.png)

- 여러번 재사용이 가능하다
- 하나의 함수는 한 가지만을 위한 것이어야 한다

# Function declaration

```jsx
function [name](param1, param2, ...param3) {
    // Function Body & Logic
}
```

- 하나의 함수는 한 가지 일만 해야된다
- function의 이름은 명령이나 동사 형태로 정해야 한다
- JavaScript에서 function은 object로 간주된다

## Parameters

- premitive parameters

  : passed by value => 메모리에 value값이 저장되어 전달

- object parameters

  : passed by reference => 메모리에 ref값이 저장되어 전달

## Default Parameters

```jsx
function showMessage(message, from = "unknown") {
  console.log(`${message} by ${from}`);
}
showMessage("Hi");
// Hi by unknown
```

⇒ 메세지만 Hi로 지정되어 있기 때문에 from은 unknown으로 출력됨

## Rest Parameters

`...` 이렇게 dot을 3개 작성하면 rest parameters가 되는데, 이는 배열 형태로 전달되게 된다

> 3개가 다 같은 결과를 출력 (dream / coding / dowon)

```jsx
function printAll(...args) {
  for (let i = 0; i < args.length; i++) {
    console.log(args[i]);
  }
}
printAll("dream", "coding", "dowon");
```

```jsx
function printAll(...args) {
  for (const arg of args) {
    console.log(arg);
  }
}
printAll("dream", "coding", "dowon");
```

```jsx
function printAll(...args) {
  args.forEach((arg) => console.log(arg));
}
printAll("dream", "coding", "dowon");
```

## Local Scope

JavaScript에서 Scope란 밖에서는 안이 보이지 않고, 안에서만 밖을 볼 수 있다

⇒ 블럭 안에 정의된 것을 블럭 밖에서는 볼 수 없지만, 블럭 밖에서 정의된 것을 블럭 안에서는 볼 수 있다

```jsx
let globalMessage = "global"; // global variable
function printMessage() {
  let message = "hello";
  console.log(message); // local variable
  console.log(globalMessage);
}
printMessage();
// hello
// global
```

## Return

모든 함수는 `return undefined;`이거나 값을 return한다.

return type이 없는 함수들은 `return undefined;`가 생략되어 있는 것이다.

> sum 함수에서는 a+b를 리턴한다

```jsx
function sum(a, b) {
  return a + b;
}
const result = sum(1, 2); // 3
console.log(`sum: ${sum(1, 2)}`); // sum: 3
```

## Early Return

빨리 return, exit하는 것

조건이 맞지 않을 때는 빨리 return을 해서 함수를 종료하고, 조건이 맞는 경우에 필요한 로직을 실행해야 한다

```jsx
function upgradeUser(user) {
  if (user.point <= 10) {
    return;
  }
  // long upgrade logic
}
```

# Function expression

- Function expression을 통해 함수가 변수로 할당되고, 다른 함수에 인자로 전달되고, 다른 함수가 리턴되는 것을 의미하는 First-class function을 가능하게 해준다.
- ✅ **function expression과 function declaration의 차이점**
  - function expression은 선언한 다음에 호출이 가능하다. 만약 선언하기 전에 호출을 하면 에러가 발생한다.
  - function declaration은 호출하고 선언해도 에러가 나지 않는다. 이는 JavaScript 엔진이 선언된 것을 가장 위로 올려주는 hoisting 때문이다.
- function에 이름이 없는 경우 ⇒ **anonymous function**
- function에 이름이 있는 경우 ⇒ **named function**

```jsx
// print(); => print 함수를 선언하기 전에 먼저 호출하면 에러 발생
const print = function () {
  // annoymous function
  console.log("print");
};
print(); // print
const printAgain = print;
printAgain(); // print
```

## Callback function

> 아래 코드에서 printYes, printNo 함수는 callback 함수에 해당

```jsx
function randomQuiz(answer, printYes, printNo) {
  if (answer === "love you") {
    printYes();
  } else {
    printNo();
  }
}

// anonymous function
const printYes = function () {
  console.log("yes!");
};

// named function
// better debugging in debugger's stack traces
// recursions
const printNo = function print() {
  console.log("no!");
};
randomQuiz("wrong", printYes, printNo); // no!
randomQuiz("love you", printYes, printNo); // yes!
```

## Arrow function

함수를 간결하게 만들어주는 것으로 항상 이름이 없는 anonymous function이다

> simplePrint! 를 출력하는 함수

```jsx
const simplePrint = () => console.log("simplePrint!");
```

```jsx
const simplePrint = function () {
  console.log("simplePrint!");
};
```

> 두 값을 입력받아 더한 값을 출력하는 함수

```jsx
const add = (a, b) => a + b;
```

```jsx
const add = function (a, b) {
  return a + b;
};
```

> Arrow function에서도 블럭 { }이 필요하면 사용 가능하지만 return을 해줘야 한다

```jsx
const simpleMultiply = (a, b) => {
  // do something more
  return a * b;
};
```

## IIFE (Immediately Invoked Function Expression)

함수를 묶어서 바로 호출하는 것으로 함수를 바로 실행하고 싶을 때 사용한다. (요즘에는 잘 쓰지 않는다.)

```jsx
(function hello() {
  console.log("IIFE");
})();
```

# Ref.

- [자바스크립트 5. Arrow Function은 무엇인가? 함수의 선언과 표현 - 프론트엔드 개발자 입문편(JavaScript ES6)](https://www.youtube.com/watch?v=e_lU39U-5bQ&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=5&t=5s)

- [함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions)

- [JavaScript Function - Declaration vs Expression](https://medium.com/@raviroshan.talk/javascript-function-declaration-vs-expression-f5873b8c7b38)
