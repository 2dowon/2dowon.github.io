---
layout: default
title: Spread Syntax
last_modified_date: 2021-03-15 14:03:26
parent: JavaScript
---

# Spread Syntax

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

Spread Syntax를 사용하면 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소 (배열 literal의 경우)로 확장하여, 0개 이상의 키-값의 쌍으로 객체로 확장시킬 수 있다. Spread Syntax를 사용하기 위해서는 배열 앞에 `...` 을 붙여서 사용할 수 있다.

Spread Syntax는 아래의 3가지 상황에서 사용한다.

1. 함수 호출 ⇒ 배열의 element를 함수의 인수로 사용하고 싶을 때
2. Array literal ⇒ 이미 존재하는 배열을 일부로 하는 새로운 배열을 생성할 때
3. Object literal ⇒ 이미 존재하는 객체를 일부로 하는 새로운 배열을 생성할 때

## 함수 호출에서의 Spread Syntax

배열의 element를 함수의 인수로 사용하고 싶을 때 사용한다.

```jsx
function myFunction(x, y, z) {
  console.log(x + y + z);
}
const args = [0, 1, 2];
myFunction(...args);
// 3
```

## Array literals에서의 spread syntax

이미 존재하는 배열을 일부로 하는 새로운 배열을 생성할 때 사용한다. Spread Syntax를 이용하면 기존에 있는 배열 안에 있는 아이템들을 하나하나씩 새로운 배열 안으로 복사할 수 있기 때문이다.

> Spread Syntax를 이용해 배열 이어붙이기

```jsx
const fruits = ["🍎", "🍌", "🍑"];
const fruits2 = ["🍉", "🍓"];
const fruits3 = ["🥝", "🍋"];
const newFruits = [...fruits, ...fruits2, ...fruits3];
console.log(newFruits);
// ["🍎", "🍌", "🍑", "🍉", "🍓", "🥝", "🍋"]
```

> Spread Syntax를 이용하지 않는다면

```jsx
const newFruits2 = fruits.concat(fruits2).concat(fruits3);
console.log(newFruits2);
```

## Object literals에서의 spread syntax

이미 존재하는 객체를 일부로 하는 새로운 객체를 생성할 때 사용한다. 즉, 새로운 객체에 기존 객체를 복사해서 가져온 후 새로운 속성을 추가하여 만들 수 있다.

> newObj에 obj 객체를 복사한 다음, 속성을 추가로 넣어서 객체로 생성한다

```jsx
const obj = { name: "A", age: 23 };
const newObj = { ...obj, height: 180 };
console.log(newObj);
// {name: "A", age: 23, height: 180}
```

> 만약 복사한 두 객체에 동일한 속성이 존재한다면 뒤에 복사한 객체의 속성의 값으로 덮어쓰여진다.

```jsx
const obj1 = { name: "A", age: 23 };
const obj2 = { name: "B", height: 180 };

const clonedObj = { ...obj1 };
console.log(clonedObj);
// {name: "A", age: 23}

const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj);
// {name: "B", age: 23, height: 180}
```

## ⚠️ Spread Syntax 주의할 점

함수를 호출할 때와 Array(배열)에서의 Spread Syntax에서 사용하는 객체는 iterable 객체여야 한다. iterable 객체가 아닌데 사용한다면 에러가 발생한다.

단, Object(객체)에서의 Spread Syntax에서는 iterable 객체가 아니어도 괜찮다.

> Uncaught TypeError: fruitObj is not iterable

fruitObj는 iterable 객체가 아니기 때문에 Spread Syntax를 Array 안에서 사용할 수 없다.

```jsx
const fruitObj = { name: "apple", emoji: "🍎" };
const fruitArray = [...fruitObj, "🍌", "🍑"];
console.log(fruitArray);
// Uncaught TypeError: fruitObj is not iterable
```

### iterable 객체

- iterable 객체는 배열을 일반화한 객체이다.
- iterable은 반복 가능하다는 뜻으로 iterable 객체는 `for..of` 반복문을 적용할 수 있다.
- ex. 배열, 문자열 등
- 아래의 경우 iterable이 아닌 유사 배열 객체이다.

  ```jsx
  let arrayLike = {
    // 인덱스와 length프로퍼티가 있음 => 유사 배열
    0: "Hello",
    1: "World",
    length: 2,
  };
  ```

  `Array.isArray()` 메서드를 이용해 true ⇒ 배열, false ⇒ 유사 배열로 확인할 수 있다.

  유사배열의 경우 배열의 메서드를 사용할 수 없다. 예를 들어, `for..of` 반복문을 적용할 수 없다.

# Ref.

- [MDN - Spread Syntax 전개 구문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

- [[JavaScript] Spread syntax (전개 구문)](https://jongbeom-dev.tistory.com/117)

- [iterable 객체](https://ko.javascript.info/iterable)

- [(JavaScript) 배열과 유사배열](https://www.zerocho.com/category/JavaScript/post/5af6f9e707d77a001bb579d2)
