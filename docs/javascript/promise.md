---
layout: default
title: Async 2. Promise
last_modified_date: 2021-02-11 18:02:19
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Async 2. Promise</div>

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

# Promise

- 비동기 코드를 간편하게 처리할 수 있는 JavaScript object로 callback을 대체해서 비동기 코드를 깔끔하게 작성할 수 있다
- 정해진 장시간의 기능을 처리하고 나서 정상적으로 기능이 수행되어졌다면 성공의 메세지와 함께 처리된 결과값을 전달해주고, 만약 기능을 수행하다가 예상치 못한 문제가 발생한다면 에러를 전달해준다
- 시간이 걸리는 일들은 promise를 통해 비동기적으로 처리하는 것이 효율적이다 (ex. 네트워크 통신, 파일 읽기 등)
- promise는 다음 중 하나의 **state 상태**를 갖는다
  - pending(수행 중) : promise 메서드를 호출했을 때 상태
  - fulfilled(성공) : promise의 callback함수(resolve)를 실행했을 때 상태
  - rejected(실패) : promise의 callback함수(reject)를 실행했을 때 상태
- [promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)의 처리 흐름

  ![promise](/assets/images/javascript/promise.png)

## Producer VS Consumer

## Producer

```jsx
new <T>(executor: (resolve: (value?: T | PromiseLike<T>) => void, reject: (reason?: any) => void) => void): Promise<T>;
```

- resolve : promise를 제대로 수행했을 때 (성공)
- reject : promise를 제대로 수행하지 못해 error가 발생하는 경우 (실패)

✅ **새로운 promise를 만들면 executor라는 callback함수(resolve, reject)가 바로 실행된다**

⇒ 이 사실을 간과하면 불필요한 네트워크 통신을 하는 경우가 생기기도 한다 (ex. user가 버튼을 눌렀을 때만 네트워크 통신이 필요함에도 불구하고, promise로 인해 바로 네트워크 통신이 실행되는 경우)

## Consumers : then, catch, finally

### then

- promise가 정상적으로 잘 수행이 되어서 마지막에 최종적으로 성공한 값인 **resolve**라는 callback함수를 통해서 전달한 값이 value의 parameter로 전달되어서 들어온다

  ```jsx
  const promise = new Promise((resolve, reject) => {
    // doing some heavy work (network, read files)
    console.log("doing something...");
    setTimeout(() => {
      resolve("dowon");
    }, 2000);
  });

  promise.then((value) => {
    console.log(value);
  });
  // dowon
  ```

- 하지만 **reject** 함수의 값은 전달되지 않으므로 'uncaught' 에러가 발생 ⇒ catch 함수가 필요

  ```jsx
  const promise = new Promise((resolve, reject) => {
    // doing some heavy work (network, read files)
    console.log("doing something...");
    setTimeout(() => {
      //resolve("dowon");
      reject(new Error("no network"));
    }, 2000);
  });

  promise.then((value) => {
    console.log(value);
  });
  // Uncaught (in promise) Error : no network
  ```

### catch

- reject라는 callback함수를 통해 에러 값을 전달함

> 성공했을 때는 value, 실패했을 때는 error를 출력

```jsx
const promise = new Promise((resolve, reject) => {
  // doing some heavy work (network, read files)
  console.log("doing something...");
  setTimeout(() => {
    //resolve("dowon");
    reject(new Error("no network"));
  }, 2000);
});

promise //
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  });
// Error : no network
```

- promise에서 then을 호출하면 다시 똑같은 promise가 리턴되기 때문에 catch를 바로 호출할 수 있다
- 그렇기에 chaining이 가능하다

### finally

- 성공과 실패와 관계없이 무조건 마지막에 호출된다
- 성공/실패와 상관없이 어떤 기능을 마지막에 수행하고 싶을 때 사용

> resolve 함수를 사용하면 dowon을 출력한 후 finally가 출력하고, reject 함수를 사용하면 Error : no network를 출력한 후 finally가 출력한다

```jsx
promise
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log("finally");
  });
```

## Promise chaining

- Promise 아래에 체인처럼 계속 이어서 `.then` 함수를 적용하는 것
- then은 값을 바로 전달해도 되고, 리턴으로 promise를 전달해도 된다

```jsx
const fetchNumber = new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 1000);
});

fetchNumber
  .then((num) => num * 2) // 1*2=2
  .then((num) => num * 3) // 2*3=6
  .then((num) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve(num - 1), 1000);
    }); // 6-1=5
  })
  .then((num) => console.log(num)); // 5
```

### **Error Handling**

> 🐓 => 🥚 => 🍳

```jsx
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve("🐓"), 1000);
  });
const getEgg = (hen) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${hen} => 🥚`), 1000);
  });
const cook = (egg) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => 🍳`), 1000);
  });

getHen()
  .then((hen) => getEgg(hen))
  .then((egg) => cook(egg))
  .then((meal) => console.log(meal));
// 🐓 => 🥚 => 🍳
```

> **TIP❗️** then 함수에서 한가지만 받아오는 경우 아래처럼 깔끔하게 적을 수 있다

```jsx
getHen()
  .then((hen) => getEgg(hen))
  .then((egg) => cook(egg))
  .then((meal) => console.log(meal));
```

```jsx
getHen().then(getEgg).then(cook).then(console.log);
```

> 바로 위 코드는 한 줄로 나오게 되는데 이는 가독성이 좋지 않기 때문에 `//` 를 이용하면 여러 줄로 나타낼 수 있다

```jsx
getHen() //
  .then(getEgg)
  .then(cook)
  .then(console.log);
```

### promise chaining을 했을 때 error를 처리하는 법

- chaining 중에 error가 발생한다면 then 다음에 바로 바로 catch를 사용해서 error를 처리할 수 있다
- getEgg에서 에러가 발생해 🥚 을 가져오지 못했지만, catch를 사용해 🥚 대신 🥖 을 리턴해서 🥖 => 🍳 가 출력될 수 있다 ⇒ 즉, promise chain이 실패하지 않는다!
- 마지막에 catch를 사용함으로써 error가 uncaught되지 않고 정상적으로 error가 발생하게 된다

```jsx
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve("🐓"), 1000);
  });
const getEgg = (hen) =>
  new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error(`error! ${hen} => 🥚`)), 1000);
  });
const cook = (egg) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => 🍳`), 1000);
  });

getHen() //
  .then(getEgg)
  .catch((error) => {
    return "🥖";
  })
  .then(cook)
  .then(console.log)
  .catch(console.log);
// 🥖 => 🍳
```

---

- 만약 then 다음에 바로 catch를 사용해 error를 처리하지 않는다면 chaining이 끝까지 수행되지 않는다
- getEgg에서 에러가 발생했는데 바로 처리하지 않았기 때문에 요리가 완성되지 않음

```jsx
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve("🐓"), 1000);
  });
const getEgg = (hen) =>
  new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error(`error! ${hen} => 🥚`)), 1000);
  });
const cook = (egg) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => 🍳`), 1000);
  });

getHen() //
  .then(getEgg)
  .then(cook)
  .then(console.log)
  .catch(console.log);
//  Error: error! 🐓 => 🥚
```

## Callback to Promise

> callback hell example

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

### callback hell을 promise로 해결하기

- promise에서는 onSuccess, onError인 callback 함수가 필요없다 (resolve, reject가 있기 때문)

```jsx
class UserStorage {
  loginUser(id, password) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (
          (id === "dowon" && password === "dream") ||
          (id === "coder" && password === "academy")
        ) {
          resolve(id);
        } else {
          reject(new Error("not found"));
        }
      }, 2000);
    });
  }
  getRoles(user) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (user === "dowon") {
          resolve({ name: "dowon", role: "admin" });
        } else {
          reject(new Error("no access"));
        }
      }, 1000);
    });
  }
}

const userStorage = new UserStorage();
const id = prompt("enter your id");
const password = prompt("enter your password");
userStorage
  .loginUser(id, password)
  .then(userStorage.getRoles)
  .then((user) => alert(`Hello ${user.name}, you have a ${user.role} role`))
  .catch(console.log);
```

# Ref.

- [자바스크립트 12. 프로미스 개념부터 활용까지 JavaScript Promise - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=JB_yU6Oe2eE&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=12)

- [자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)
