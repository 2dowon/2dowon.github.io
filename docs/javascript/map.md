---
layout: default
title: JS 자료구조 - ES6 Map
last_modified_date: 2021-11-09 23:11:60
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">JS 자료구조 - ES6 Map</div>

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

일반적으로 JS에서 사용하는 자료구조는 객체(Object)와 배열(Array)이다. 이후 ES6에서 새로운 자료구조인 맵(Map)과 셋(Set)이 등장하게 되었고, 이중에서 Map을 정리하려고 한다.

# 자료구조 Map

- Map은 자바스크립트의 key-value 쌍으로 데이터를 저장한다.
  이 점에서 Object와 유사하다고 생각할 수 있다. 하지만 Object의 key 는 string 과 symbol(ES6) 만 가능하지만, map 은 어떤 값도 가능하다는 차이점을 가지고 있다.
- key 를 사용해서 value 를 get, set 할 수 있다.
- key 들은 중복될 수 없다. 하나의 key 에는 하나의 value 만 존재한다.
  (이 점에서 개인적으로 Python의 Dictionary와 비슷한 자료구조라 생각하고 있다.)

## 주요 메서드와 프로퍼티

- `new Map()` : 맵을 만들 때 사용
- `map.set(key, value)` : `key`를 이용해 `value`를 저장
- `map.get(key)` :  `key`에 해당하는 값을 반환. `key`가 존재하지 않으면 `undefined`를 반환.
- `map.has(key)` : `key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환
- `map.delete(key)` : `key`에 해당하는 값을 삭제
- `map.clear()` : 맵 안의 모든 요소를 제거
- `map.size` : 요소의 개수를 반환

```jsx
let map = new Map();

map.set("1", "str1"); // 문자형 키
map.set(1, "num1"); // 숫자형 키
map.set(true, "bool1"); // 불린형 키

console.log(map.get(1)); // 'num1'
console.log(map.get("1")); // 'str1'
console.log(map.size); // 3
```

## 맵의 요소에 반복 작업하기

- `map.keys()` : 각 요소의 키를 모은 반복 가능한(iterable, 이터러블) 객체를 반환
- `map.values()` : 각 요소의 값을 모은 이터러블 객체를 반환
- `map.entries()` : 요소의 `[키, 값]`을 한 쌍으로 하는 이터러블 객체를 반환. 이 이터러블 객체는 `for..of`반복문의 기초로 쓰인다

```jsx
let recipeMap = new Map([
  ["cucumber", 500],
  ["tomatoes", 350],
  ["onion", 50],
]);

for (let vegetable of recipeMap.keys()) {
  console.log(vegetable); // cucumber, tomatoes, onion
}

for (let amount of recipeMap.values()) {
  console.log(amount); // 500, 350, 50
}

for (let entry of recipeMap) {
  // recipeMap.entries()와 동일
  console.log(entry); // cucumber,500 ...
}
```

## Object.entries: 객체를 맵으로 바꾸기

> 각 요소가 키-값 쌍인 배열을 Map으로 만들기

```jsx
let map = new Map([
  ["1", "str1"],
  [1, "num1"],
  [true, "bool1"],
]);

console.log(map.get("1")); // str1
```

> 일반 Object(객체)를 Map으로 만들기

먼저 Object를 [Object.entries(obj)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)를 활용해서 키-값 쌍인 배열로 만든 후 위 방법처럼 Map으로 만든다.

```jsx
let obj = {
  name: "John",
  age: 30,
};

let map = new Map(Object.entries(obj));

console.log(map.get("name")); // John
```

## Object.fromEntries: 맵을 객체로 바꾸기

> 각 요소가 키-값 쌍인 배열을 객체로 만들기

```jsx
let prices = Object.fromEntries([
  ["banana", 1],
  ["orange", 2],
  ["meat", 4],
]);

console.log(prices); // { banana: 1, orange: 2, meat: 4 }
console.log(prices.orange); // 2
```

> 자료가 Map에 저장되어 있을 때 바로 객체로 만들기

자료가 Map에 저장이 되어있으면 먼저 `map.entries()` 를 이용해 키-값 쌍인 배열로 만든 후 이를 Object로 만들어줘야 한다

```jsx
let map = new Map();
map.set("banana", 1);
map.set("orange", 2);
map.set("meat", 4);

let obj = Object.fromEntries(map.entries());
// obj = { banana: 1, orange: 2, meat: 4 }

console.log(obj.orange); // 2
```

# Ref.

- [맵과 셋](https://ko.javascript.info/map-set)

- [[JS #5] ES6 Map(), Set()](https://medium.com/@hongkevin/js-5-es6-map-set-2a9ebf40f96b#b0b1)
