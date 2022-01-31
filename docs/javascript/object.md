---
layout: default
title: Object
last_modified_date: 2021-02-09 18:02:15
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Object</div>

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

- Object는 JavaScript의 데이터 타입 중 하나
- 거의 모든 objects는 Object의 instances이다
- Object를 이용하면 데이터를 효율적으로 관리할 수 있다.
- object는 key와 value의 집합체

### `object = { key : value };`

- key : 우리가 접근할 수 있는 변수/property
- value : 그 property가 가지고 있는 값

> key : name, age / value : dowon, 27

```jsx
const dowon = { name: "dowon", age: 27 };
```

## object를 만드는 방법

- object literal syntax

  : **`const obj1 = {};`** ⇒ { }를 이용해서 만드는 방법

- object constructor syntax

  : **`const obj2 = new Object();`** ⇒ class를 이용해서 new를 붙여 만드는 방법

### object의 property를 중간에 추가하거나 삭제가 가능

JavaScript는 dynamically typed language이다. 다시 말해, JavaScript는 동적으로 type이 runtime(프로그램이 작동할 때) 때 결정된다. 따라서 object의 property를 중간에 추가하거나 삭제하는 것이 가능하다.

> "hasjob" property를 중간에 추가 및 삭제

```jsx
const dowon = { name: "dowon", age: 27 };
dowon.hasJob = true;
console.log(dowon.hasJob); // true

delete dowon.hasJob;
console.log(dowon.hasJob); // false
```

## Computed properties

### object 안에 있는 data에 접근하는 방법

**1. dot`.` 을 이용해 접근하는 방법**

- 코딩하는 그 순간 key에 해당하는 그 값을 받아오고 싶을 때 사용

```jsx
console.log([dowon.name]); // dowon
```

**2. Computed properties : 배열에서 데이터를 받아 접근하는 방법**

- 이때 key는 string type으로 지정해서 받아와야 한다
- Computed properties는 정확하게 어떤 key가 필요한지 모를 때, runtime에서 결정될 때 사용

```jsx
console.log(dowon["name"]); // dowon
console.log(dowon[name]); // undefined
```

> printValue 함수에서 어떤 obj와 key를 받을지 모르는 경우에는 Computed properties를 이용해야 한다. `console.log(obj.key);` 는 key라는 property를 찾는 것이므로 적절하지 않다. ⇒ 동적으로 key를 받을 때 유용하다

```jsx
function printValue(obj, key) {
  console.log(obj[key]);
}
printValue(dowon, "name"); // dowon
printValue(dowon, "age"); // 26
```

## Property value shorthand

key와 value의 이름이 동일하다면 생략이 가능하다

```jsx
const person4 = makePerson("dowon", 26);
console.log(person4);
function makePerson(name, age) {
  return {
    name: name,
    age: age,
  };
}
```

> key와 value의 이름이 name과 age로 동일하기 때문에 생략 가능

```jsx
const person4 = makePerson("dowon", 26);
console.log(person4);
function makePerson(name, age) {
  return {
    name,
    age,
  };
}
```

## Constructor function

JavaScript에 클래스가 없었을 때는 function을 이용해서 object를 생성했다. 이처럼 다른 계산을 하지 않고 순수하게 object를 생성하는 함수의 경우 다음과 같은 규칙을 지켜야 한다.

- 대문자로 함수의 이름을 시작해야 한다
- return 대신 this를 사용한다
- 함수지만 호출할 때도 class 처럼 new를 붙여서 호출해야 한다

```jsx
console.log(person4);
function Person(name, age) {
  // this = {};
  this.name = name;
  this.age = age;
  // return this;
}
const person4 = new Person("dowon", 26);
// Person {name: "dowon", age: 26}
```

## in operator

해당하는 object 안에 key가 있는지 없는지를 간단히 확인할 수 있다 ⇒ true/false로 결과를 알려준다

```jsx
const dowon = { name: "dowon", age: 27 };
console.log("name" in dowon); // true
console.log("age" in dowon); // true
console.log("random" in dowon); // false
console.log(dowon.random); // undefined
```

## for..in vs for..of

### for (key in obj)

모든 key들을 받아와서 일을 처리하고 싶을 때 사용한다

> dowon이 가지고 있는 key들이 블럭{ }을 돌 때마다 key라는 지역 변수에 할당된다. 따라서 console.log로 출력을 하면 dowon 안에 있는 모든 key들이 출력된다.

```jsx
for (let key in dowon) {
  console.log(key);
}
// name
// age
```

### for (value of iterable)

array에 있는 모든 값들이 value에 할당되면서 블럭{ }안에서 순차적으로 출력하거나 값을 계산할 수 있다

```jsx
const array = [1, 2, 3, 4, 5];
for (let value of array) {
  console.log(value);
}
// 1, 2, 3, 4, 5
```

## object를 복제/복사하는 방법

```jsx
const user = { name: "dowon", age: "26" };
const user2 = user;
user2.name = "coder";
console.log(user); // { name: "coder", age: "26" }
```

### 1. for...in (old way)

⇒ object를 수동적으로 복제하는 방법

```jsx
const user = { name: "dowon", age: "26" };
const user3 = {};
for (let key in user) {
  user3[key] = user[key];
}
console.log(user3); // { name: "dowon", age: "26" }
```

### 2. [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

`Object.assign(target, ...sources)`

```jsx
const user = { name: "dowon", age: "26" };
const user4 = {};
Object.assign(user4, user);
console.log(user4); // { name: "dowon", age: "26" }

// object를 만들면서 동시에 assign를 사용 가능
const user5 = Object.assign({}, user);
console.log(user5);
```

⚠️ assign은 뒤에 있는 아이일수록 앞에 같은 property가 있다면 계속 값을 덮어씌우기 때문에 유의해야 한다

> mixed.color의 경우 fruit1의 color는 무시되고, fruit2의 color인 blue가 출력된다. (color라는 같은 property를 가지고 있는데, fruit2가 더 뒤에 있기 때문)

```jsx
const fruit1 = { color: "red" };
const fruit2 = { color: "blue", size: "big" };
const mixed = Object.assign({}, fruit1, fruit2);
console.log(mixed.color); // blue
console.log(mixed.size); // big
```

# Ref.

- [자바스크립트 7. 오브젝트 넌 뭐니? - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=1Lbr29tzAA8&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=7)

- [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
