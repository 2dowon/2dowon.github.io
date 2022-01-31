---
layout: default
title: Async 1. Callback
last_modified_date: 2021-02-11 18:02:30
parent: JavaScript
---

# Async 1. Callback

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

# 동기와 비동기

## synchronous 동기

- JavaScript는 synchronous 동기적이다
- hoisting 이후부터 코드가 (동기적으로) 순서대로 하나 하나 실행된다는 뜻

  (hoisting 호이스팅 : var, function 선언이 자동으로 제일 위로 올라가는 것)

## asynchronous 비동기

- 언제 코드가 실행될지 예측할 수 없다

**setTimeout**

- webAPI로 지정한 시간이 지나면 지정한 callback 함수를 호출하는 것

> 브라우저API는 응답을 기다리지 않고 다음 코드를 먼저 수행하기 때문에 setTimeout을 기다리지 않고, 다음 코드인 '3'을 먼저 출력하고 1초 후에 '2'를 출력한다

```jsx
console.log("1");
setTimeout(function () {
  console.log("2"); // callback 함수
}, 1000);
console.log("3");
// 1 / 3 / 2
```

> arrow를 이용하여 간단하게 표시 가능

```jsx
console.log("1");
setTimeout(() => console.log("2"), 1000);
console.log("3");
```

# callback

- callback함수는 지금 말고 나중에 부를 때 실행하는 함수이다.
- Synchronous callback ⇒ 즉각적으로 동기적으로 사용하는 callback
- Asynchronous callback ⇒ 나중에 언제 실행될지 예측할 수 없는 callback

## Synchronous callback

- 즉각적으로 동기적으로 사용하는 callback

> Synchronous callback 예시

```jsx
function printImmediately(print) {
  print();
}
console.log("1");
setTimeout(() => console.log("2"), 1000);
console.log("3");
printImmediately(() => console.log("hello"));
// 1 / 3 / hello / 2
```

printImmediately 함수 선언은 위치에 상관없이 hoisting에 의해 맨 위에서 인식하게 될 것이고, 그 이후에 '1' 출력, '2'를 브라우저에 보내고, '3'을 출력, printImmediately 함수를 실행해 'hello'를 출력하고 마지막으로 1초 후에 '2'를 출력하여 위와 같은 결과가 나온다

## Asynchronous callback

- 나중에 언제 실행될지 예측할 수 없는 callback

> Asynchronous callback 예시

```jsx
function printImmediately(print) {
  print();
}
function printWithDelay(print, timeout) {
  setTimeout(print, timeout);
}
console.log("1");
setTimeout(() => console.log("2"), 1000);
console.log("3");
printImmediately(() => console.log("hello"));
printWithDelay(() => console.log("async callback"), 2000);
// 1 / 3 / hello / 2 / async callback
```

printImmediately와 printWithDelay 함수 선언은 위치에 상관없이 hoisting에 의해 맨 위에서 인식하게 될 것이고, 그 이후에 '1' 출력, '2'를 브라우저에 보내고, '3'을 출력, printImmediately 함수를 실행해 'hello'를 출력하고, printWithDelay 함수를 실행하는데 이는 2초 후에 실행되기 때문에, 1초 후에 '2'를 먼저 출력하고 그 후 2초가 지나 'asynce callback'이 출력되어 위와 같은 결과가 나온다

## Callback Hell

- callback 함수를 계속 묶어가면서 callback 함수 안에서 다른 callback 함수를 부르는 것을 계속하는 경우

### callback Hell의 문제점

- 가독성이 떨어진다
- 비즈니스로직을 한 번에 이해하기 어렵다
- error가 발생하거나, 디버깅을 해야될 때 굉장히 어렵다
- 유지보수도 어렵다

> Callback Hell Example

```jsx
class UserStorage {
  loginUser(id, password, onSuccess, onError) {
    setTimeout(() => {
      if (
        (id === "dowon" && password === "dream") ||
        (id === "coder" && password === "academy")
      ) {
        onSuccess(id);
      } else {
        onError(new Error("not found"));
      }
    }, 2000);
  }
  getRoles(user, onSuccess, onError) {
    setTimeout(() => {
      if (user === "dowon") {
        onSuccess({ name: "dowon", role: "admin" });
      } else {
        onError(new Error("no access"));
      }
    }, 1000);
  }
}

const userStorage = new UserStorage();
const id = prompt("enter your id");
const password = prompt("enter your password");
userStorage.loginUser(
  id,
  password,
  (user) => {
    userStorage.getRoles(
      user,
      (userWithRole) => {
        alert(
          `Hello ${userWithRole.name}, you have a ${userWithRole.role} role`
        );
      },
      (error) => {
        console.log(error);
      }
    );
  },
  (error) => {
    console.log(error);
  }
);
```

# Ref.

- [자바스크립트 11. 비동기 처리의 시작 콜백 이해하기, 콜백 지옥 체험 😱 JavaScript Callback - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=s1vpVCrT8f4&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=11)
