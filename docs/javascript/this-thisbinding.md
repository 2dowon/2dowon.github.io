---
layout: default
title: this와 this 바인딩
last_modified_date: 2021-03-01 20:03:34
parent: JavaScript
---

# this와 this 바인딩

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

# this

- 대부분의 객체지향 언어에서 this는 클래스로 생성한 인스턴스 객체를 의미한다. 따라서 이 때 this는 클래스에서만 사용할 수 있다.
- ✅ 반면, **JavaScript에서 this는 어디서든 사용할 수 있다.**
- JavaScript에서 this는 기본적으로 함수를 호출할 때 결정된다. 즉, 함수를 어떤 방식으로 호출하느냐에 따라 값이 달라진다.

# JavaScript - this 의 특징

## 1. JavaScript에서 this는 어디서든 사용할 수 있다.

### 전역공간에서 this

- 전역공간에서 this는 전역 객체를 가리킨다.
- 브라우저 환경에서 전역 객체는 window이고, Node.js 환경에서는 global이다.

### 메서드와 this

메서드 내부에서 this 키워드를 사용하면 객체에 접근할 수 있다.

> `this.name` 에서 this는 현재 객체인 user를 의미한다

```jsx
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(this.name);
  },
};

user.sayHi(); // John
```

## 2. JavaScript에서 this 값은 런타임에 결정된다.

JavaScript에서 this는 기본적으로 함수를 호출할 때 결정된다. 즉, 함수를 어떤 방식으로 호출하느냐에 따라 값이 달라진다. 예를 들어, 같은 함수라도 다른 객체에서 호출했다면 this가 참조하는 값은 달라진다.

> user.f와 admin.f 모두 동일한 함수 sayHi를 사용했지만, 서로 다른 객체에서 사용하고 있기 때문에 this가 참조하는 값이 달라 다른 결과가 나온다

```jsx
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert(this.name);
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin["f"](); // Admin (점과 대괄호는 동일하게 동작함)
```

## 3. 객체 메서드를 콜백으로 전달하면 this 정보는 사라진다

객체 메서드가 객체 내부가 아닌 다른 곳에 전달되어 호출되면 (어떤 클래스 안에 있는 함수를 다른 콜백으로 전달하면) 해당 함수가 포함되어져 있는 클래스의 정보인 this 정보가 사라지는 문제가 발생한다.

> user.sayHi는 객체 내부가 아닌 다른 곳에 전달되어 호출되었기 때문에 user의 this 정보는 사라지고 setTimeout의 this 값은 브라우저 환경의 전역객체인 window가 된다. window.firstName은 없으므로 undefined가 출력된다.

```jsx
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  },
};

setTimeout(user.sayHi, 1000); // Hello, undefined!
```

따라서 this 정보가 사라지는 문제를 해결하기 위해서는 **this 바인딩**이 필요하다.

# this 바인딩

JavaScript에서 this 값은 런타임 때 결정되지만, this 바인딩을 이용하면 this가 무엇을 가리키는지 명시적으로 알려주기 때문에 this 정보가 사라지는 문제를 해결할 수 있다.

## 1. arrow function

- arrow function을 이용하면 가장 간단하게 this바인딩을 할 수 있다.
- 하지만 이 방법은 약간의 취약성이 있다. 예를 들어, 아래 예시에서 1초가 지나기 전에 user를 변경하면 변경된 값이 바인딩되어서 결국 변경된 객체의 메서드를 호출하게 된다.

> `setTimeout(user.sayHi, 1000);` 에 arrow function을 이용해서 this 바인딩함

```jsx
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  },
};

setTimeout(() => user.sayHi(), 1000); // Hello, John!
```

## 2. bind

- bind는 JavaScript의 내장 함수로 this 값을 고정한 새로운 함수를 만들어준다
- bind의 기본 문법

  ```jsx
  let boundFunc = func.bind(context);
  ```

- bind를 이용할 경우 this 바인딩이 필요할 때마다 적어줘야 한다.

> funcUser는 func 함수에 this를 user로 고정한 것이다. 따라서 funcUser를 호출했을 때 this 값은 window가 아닌 user가 되고, user.firstName인 John을 출력한다.

```jsx
let user = {
  firstName: "John",
};

function func() {
  alert(this.firstName);
}

let funcUser = func.bind(user);
funcUser(); // John
```

# Ref.

- [메서드와 'this'](https://ko.javascript.info/object-methods)

- [함수 바인딩](https://ko.javascript.info/bind#ref-554)
