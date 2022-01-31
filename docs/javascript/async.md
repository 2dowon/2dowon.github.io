---
layout: default
title: Async 3. async & await
last_modified_date: 2021-02-12 15:02:46
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Async 3. async & await</div>

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

# async & await

- Promise를 동기식으로 코드를 순서대로 작성하는 것처럼 간편하게 작성을 도와주는 API
- async & await는 새롭게 추가된 것이 아니라, 기존에 존재하는 Promise 위에 조금 더 간편한 API를 제공하는 것이다. 이를 syntactic sugar라고 한다. (ex. class)
- Promise chaning을 계속 사용하다보면 가독성이 떨어지기 때문에 async & await를 사용하면 읽기 좋은 코드를 작성할 수 있다

### 비동기 처리를 반드시 해야하는 이유

- JavaScript는 코드가 순서대로 하나씩 실행되는 동기적 언어이기 때문에, 시간이 오래 걸리는 코드를 비동기 처리를 전혀 하지 않으면 다음 코드에 문제가 발생할 수 있다
- 예를 들어, 서버에서 data를 받아와서 웹페이지에 출력한다고 할 때, data를 받아 오는데 10초가 걸린다고 한다. 이 때 비동기 처리를 하지 않으면 데이터를 가져오는 10초 동안 유저는 빈 페이지를 보게 된다. 따라서 promise를 통해 then이라는 콜백 함수를 등록해 나중에 유저의 data가 준비되는대로 등록된 콜백 함수를 실행하도록 만든다.

## async

- promise를 간편하게 쓸 수 있는 syntactic sugar
- 오래 걸리는 일을 비동기적으로 처리하지 않으면 JavaScript 엔진은 동기적으로 코드를 수행하기 때문에 그 코드가 끝날 때까지 다른 코드를 실행하지 않는다. ⇒ 따라서 비동기적으로 처리하기 위해 promise를 사용
- 하지만 promise에서 resolve, reject를 호출하지 않으면 promise의 상태는 pending (진행중)으로 남아 있게 된다. 따라서 promise에서는 반드시 resolve나 reject를 호출해서 성공 또는 실패의 상태로 만들어야 결과를 볼 수 있다.

### 함수 앞에 async를 붙여주면 자동적으로 함수가 promise로 변환된다

> promise

```jsx
function fetchUser() {
  return new Promise((resolve, reject) => {
    //do network request in 10 secs...
    resolve("dowon");
  });
}

const user = fetchUser();
user.then(console.log);
console.log(user);
```

> async

```jsx
async function fetchUser() {
  //do network request in 10 secs...
  return "dowon";
}

const user = fetchUser();
user.then(console.log);
console.log(user);
```

## await

- await 함수는 async가 붙은 함수 안에서만 사용 가능하다

> delay, getApple, getBanana

```jsx
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(1000);
  return "🍎";
}

async function getBanana() {
  await delay(1000);
  return "🍌";
}
```

> await을 사용하지 않은 pickFruits

- 콜백지옥과 비슷 ⇒ 가독성이 좋지 않다

```jsx
function pickFruits() {
  return getApple().then((apple) => {
    return getBanana().then((banana) => `${apple} + ${banana}`);
  });
}

pickFruits().then(console.log); // 🍎 + 🍌
```

> await을 사용한 pickFruits

```jsx
async function pickFruits() {
  const apple = await getApple();
  const banana = await getBanana();
  return `${apple} + ${banana}`;
}

pickFruits().then(console.log); // 🍎 + 🍌
```

### error handling

async에서는 try/catch를 통해 error를 처리할 수 있다

```jsx
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(1000);
  throw "error"; // error를 발생시킴
  return "🍎";
}

async function getBanana() {
  await delay(1000);
  return "🍌";
}

async function pickFruits() {
  try {
    const apple = await getApple();
    const banana = await getBanana();
    return `${apple} + ${banana}`;
  } catch (error) {
    console.log(error);
  }
}

pickFruits().then(console.log);
```

### await 병렬 처리

- await을 여러 개 쓰다보면 하나의 await을 처리한 후, 다음 await을 처리하는데 두 함수가 서로 연관이 없다면 매우 비효율적이다. 이 때 promise 함수를 이용하면 바로 바로 함수가 실행될 수 있게 만들 수 있다.
- await 함수들이 동시에 기다렸다가 한 번에 출력이 가능해진다

> 사과 1초, 바나나 1초를 동시에 기다렸다가 1초 후에 출력

```jsx
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(1000);
  return "🍎";
}

async function getBanana() {
  await delay(1000);
  return "🍌";
}

async function pickFruits() {
  const applePromise = getApple();
  const bananaPromise = getBanana();
  const apple = await applePromise;
  const banana = await bananaPromise;
  return `${apple} + ${banana}`;
}

pickFruits().then(console.log); // 🍎 + 🍌
```

> promise를 사용하지 않으면 사과 1초, 바나나 1초를 각각 기다려 2초 후에 출력

```jsx
async function pickFruits() {
  const apple = await getApple();
  const banana = await getBanana();
  return `${apple} + ${banana}`;
}
```

## useful Promise APIs

### Promise.all

- promise 배열을 전달하게 되면 모든 promise들이 병렬적으로 다 받을 때까지 모아준다

> 사과와 바나나를 모은 후에 join을 이용해 +로 묶어서 출력

```jsx
function pickAllFruits() {
  return Promise.all([getApple(), getBanana()]).then(
    (fruits) => fruits.join(" + ") // 배열을 string으로 묶어줌
  );
}
pickAllFruits().then(console.log); // 🍎 + 🍌
```

### Promise.race

어떤 것이든 상관없고 맨 첫 번째 것만 받아오고 싶을 때 사용

> 사과는 2초가 걸리고, 바나나는 1초가 걸린다고 가정

```jsx
function pickOnlyOne() {
  return Promise.race([getApple(), getBanana()]);
}
pickOnlyOne().then(console.log); // 🍌
```

# Ref.

- [자바스크립트 13. 비동기의 꽃 JavaScript async 와 await 그리고 유용한 Promise APIs - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=aoQSOZfz3vQ&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=13)

- [자바스크립트 async와 await](https://joshua1988.github.io/web-development/javascript/js-async-await/)
