---
layout: default
title: 여러개의 화살표 함수 (=>)
last_modified_date: 2022-03-19 23:02:00
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">여러개의 화살표 함수 (=>)</div>

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

# 여러개의 화살표 함수 (⇒)

화살표 함수는 ES6에서 도입된 문법으로 함수를 좀 더 간결하고 직관적으로 작성할 수 있어서 자주 사용하고 있다. 자주 사용하고 있었지만, 화살표 함수는 항상 하나만 사용했었는데 이번에 화살표 함수를 여러개 중첩으로 사용하는 코드를 처음 보게 되어서 한 번 정리해두려고 한다!

# 함수 표현 (**function expression**)

## 전통적인 함수 (function)

```jsx
const sum = function (a, b) {
  return a + b;
};
```

## 화살표 함수 (**arrow function**)

```jsx
const add = (a, b) => {
  return a + b;
};
```

> 화살표 함수의 특징 (화살표 함수와 전통적인 함수의 차이점)

- [this](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)나 [super](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/super)에 대한 바인딩이 없고, [methods](https://developer.mozilla.org/ko/docs/Glossary/Method) 로 사용될 수 없다.
- [new.target](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new.target)키워드가 없다.
- 일반적으로 스코프를 지정할 때 사용하는 [call](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call), [apply](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply), [bind](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) methods를 이용할 수 없다.
- 생성자[(Constructor)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes/constructor)로 사용할 수 없다.

# 여러개의 화살표 함수

### 2개의 화살표 함수

```jsx
const add = (a) => (b) => {
  return a + b;
};
```

- 처음 봤을 때는 뭐지 싶게 생겼는데, 아래처럼 전통적인 함수 방식으로 풀어서 쓰면 이해하기 쉽다!
  ```jsx
  const add = function (a) {
    return function (b) {
      return a + b;
    };
  };
  ```

### 화살표 함수 사용법

```jsx
const add = (a) => (b) => {
  return a + b;
};

add(2)(3); // 5

const add2 = add(2);
add2(3); // 5
```

### 🤔  왜 굳이 화살표 함수 여러개를 쓸까....?

- 위 예시를 보고 처음 든 생각은 왜 굳이 화살표 함수를 여러개 쓸까였다.
- 예를 들어서, a와 b를 더하는 add 함수를 만들기 위해서 아래처럼 a와 b 두 개의 인자를 한 번에 받으면 되는 것이 아닌가 그런 생각이 들었기 때문이다.

  ```jsx
  const add = (a, b) => {
    return a + b;
  };

  add(2, 3); // 5
  ```

- 근데 생각하다보니까 이 경우는 여러개의 화살표 함수의 설명 예시를 워낙 간단한 예시로 만들었기 때문인 것 같다. 예를 들어서 a와 b 두 개의 인자를 한번에 받는 경우 add 함수를 쓸 때부터 a와 b 두 개의 인자를 모두 알고 있어야 한다. 하지만 **두 개의 화살표 함수를 사용한다면 첫 번째 (외부) 함수 호출이 두 번째 (내부) 함수를 반환**하기 때문에 add 함수를 호출할 때 첫번째 인자 a를 넣어서 리턴된 값을 이용할 수 있다.
- 아래 예시를 다시 살펴보면 `add2`는 첫번째 함수를 호출해서 두번째 함수를 반환한다. 따라서 add2에 두번째 인자를 넣어주게 되면 `add2`를 만들때 넣었던 첫 번째 인자 a인 2와 `add2`에 사용된 두번째 인자 b인 3이 합쳐져서 5라는 결과가 나오는 것이다.

  ```jsx
  const add = (a) => (b) => {
    return a + b;
  };

  const add2 = add(2);
  add2(3); // 5
  ```

# Ref.

- [화살표 함수 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

- [자바스크립트에서 화살표가 2번 연속으로 나오는 경우는 어떻게 해석해야하나요?](https://hashcode.co.kr/questions/7544/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90%EC%84%9C-%ED%99%94%EC%82%B4%ED%91%9C%EA%B0%80-2%EB%B2%88-%EC%97%B0%EC%86%8D%EC%9C%BC%EB%A1%9C-%EB%82%98%EC%98%A4%EB%8A%94-%EA%B2%BD%EC%9A%B0%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%95%B4%EC%84%9D%ED%95%B4%EC%95%BC%ED%95%98%EB%82%98%EC%9A%94)

- [자바 스크립트에서 여러 개의 화살표 기능은 무엇을 의미합니까?](http://daplus.net/javascript-%EC%9E%90%EB%B0%94-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%97%90%EC%84%9C-%EC%97%AC%EB%9F%AC-%EA%B0%9C%EC%9D%98-%ED%99%94%EC%82%B4%ED%91%9C-%EA%B8%B0%EB%8A%A5%EC%9D%80-%EB%AC%B4%EC%97%87/)
