---
layout: default
title: Date types, Dynamic typing
last_modified_date: 2021-02-08 15:02:26
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Date types, Dynamic typing</div>

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

# 변수 선언

- JavaScript에서 변수를 선언하는 방법에는 mutable data type의 변수를 선언하는 **let**과 immutable data type 변수를 선언하는 **const**가 있다.
- **Immutable data types**: 기본 자료형(primitive types), frozen objects (i.e. object.freeze())
- **Mutable data types**: all objects by default are mutable in JS
- 지역 변수(local variable)와 전역 변수(global variable)

  - 지역변수 local variable : 지역 변수는 변수가 위치하는 block scope인 local scope 범위를 갖게된다. 따라서 블럭 안에 선언한 변수는 블럭 밖에서는 출력할 수 없다.

  - 전역 변수 global variable : 전역 변수는 global scope의 범위를 갖게 된다. 따라서 블럭 안이나 밖에서나 출력할 수 있다. 하지만 항상 메모리에 탑재하므로 최소한 부분에서만 사용하는 것이 좋다.

## let

- mutable data type
- 변수를 선언하고, 값을 할당함 (그러지 않으면 에러가 발생)

## Var (사용 X)

**var를 사용하면 안되는 2가지 이유**

1. 변수를 선언하고, 값을 할당하는 것이 정상적인 방법이지만, var에서는 순서를 바꿔도 에러가 나오지 않음 (var hoisting이 발생하기 때문)

   ✅ **hoisting** : 어디에 선언했는지와 상관없이 항상 제일 위로 선언을 끌어올려주는 것

2. block scope가 존재하지 않음 ⇒ 블럭 안에 넣은 변수를 블럭 밖에서 출력해도 출력된다

## Constants

- immutable data type (변경이 불가능한 데이터 타입)
- 값이 변경될 이유가 없다면 가능한 const로 프로그램을 작성하는 것이 좋다
  1. security 보안상의 이유
  2. thread safety 다양한 thread가 동시에 변경하는 것이 위험하기 때문
  3. reduce human mistakes 실수 방지

# Variable types

- Variable types에는 primitive types, object, function이 있다
- primitive types은 value가 메모리에 저장되고, object는 너무 크기 때문에 ref가 메모리에 저장된다

## primitive types 기본 자료형

더 이상의 작은 단위로 나누어지지 않는 한 가지의 아이템

ex. number, string, boolean, null, undefiedn, symbol

### NaN

Not a number 숫자가 아닌 것을 숫자로 나눌 때 생기는 에러

```jsx
const nAn = "not a number" / 2;
console.log(nAn);
```

### String

한 개의 글자나 여러 개의 글자 모두 string type

string끼리는 서로 붙여서 사용 가능

```jsx
const char = "c";
const brendan = "brendan";
const greeting = "hello" + brendan;
console.log(`value: ${greeting}, type: ${typeof greeting}`);
const helloBob = `hi ${brendan}!`; //template literals (string)
console.log(`value: ${helloBob}, type: ${typeof helloBob}`);
```

### Boolean

true와 false로 이루어져 있음

- true: any other value
- false: 0, null, undefined, NaN.

### Null

명확하게 텅텅 비어있는 값이라고 지정해주는 것 ⇒ null로 값이 할당되어져 있음

### Undefined

아무것도 값이 지정되어 있지 않은 상태

`let x;` 이렇게 아무 값도 할당하지 않아도 되고, `let x = undefined;`라고 해도 똑같은 undefined

### Symbol

나중에 다른 자료 구조에서 고유한 자료 식별자가 필요하거나, 동시다발적으로 일어나는 코드에서 우선순위를 주고 싶을 때 식별자가 필요할 때 쓰는 것

- 동일한 string을 가지고 다른 symbol을 만드는 것이 가능하다.
- 주어진 string에 맞는 symbol을 만들고 싶다면, for문을 이용해야 된다.
- symbol은 바로 출력하면 에러가 나기 때문에 `.description`을 이용해서 string으로 바꾼 후 출력해야 한다.

```jsx
const symbol1 = Symbol("id");
const symbol2 = Symbol("id");
console.log(symbol1 === symbol2); //false
const gSymbol1 = Symbol.**for**("id");
const gSymbol2 = Symbol.**for**("id");
console.log(gSymbol1 === gSymbol2); //true
console.log(`value: ${symbol1**.description**}, type: ${typeof symbol1}`);
```

## Object 객체

하나의 아이템들을 여러 개로 묶어서 한 단위로, 하나의 박스로 관리할 수 있게 해준다

```jsx
const dowon = { name: "dowon", age: 26 };
dowon.age = 21;
console.log(dowon.age);
```

- 변수 name, age는 아무것도 설명하지 못하지만, object로 설명할 수 있고 object의 요소들은 바꿀 수 있음
- dowon은 const로 선언되어서 다른 object로 변경할 수 없다. 하지만 dowon의 age는 object의 요소이기 때문에 26에서 21로 수정할 수 있다

## function 함수

- function도 다른 데이터 타입처럼 변수에 할당이 가능
- 함수의 파라미터, 인자로도 전달 가능
- fuction을 리턴 가능

# Dynamic typing

Dynamic typing은 data type이 유연하게 변하는 것이다. JavaScript는 런타임하면서 타입이 결정되기 때문에 Dynamic typing이 자주 발생하고, 런타임 에러도 자주 발생한다. 이 때문에 Type Script가 나오게 된다.

```jsx
let text = "hello";
console.log(`value: ${text}, type: ${typeof text}`);
// value : hello, type: string
```

```jsx
text = 1;
console.log(`value: ${text}, type: ${typeof text}`);
// value : 1, type: number
```

```jsx
text = "7" + 5;
console.log(`value: ${text}, type: ${typeof text}`);
// value : 75, type: string
```

```jsx
text = "8" / "2";
console.log(`value: ${text}, type: ${typeof text}`);
// value : 4, type: number
```

# Ref.

- [자바스크립트 3. 데이터타입, data types, let vs var, hoisting - 프론트엔드 개발자 입문편 (JavaScript ES5+)](https://www.youtube.com/watch?v=OCCpGh4ujb8&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=3)

- [자바스크립트의 자료형](https://developer.mozilla.org/ko/docs/Web/JavaScript/Data_structures)

- [JavaScript Scope](http://jun.hansung.ac.kr/CWP/Javascript/JavaScript%20Scope.html)
