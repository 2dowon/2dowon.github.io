---
layout: default
title: JSON
last_modified_date: 2021-02-11 12:02:00
parent: JavaScript
---

# JSON

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

# HTTP

- Hypertext Transfer Protocal
- Client와 Server가 서로 어떻게 Hypertext를 주고 받는지를 규약한 Protocal의 하나
- Client가 request를 보내면 Server가 response해서 다시 Client에게 전달한다
- Hypertext에는 하이퍼링크, 문서, 이미지 모두를 포함한다

# AJAX

- [AJAX](https://developer.mozilla.org/ko/docs/Web/Guide/AJAX/Getting_Started) = Asynchronous JavaScript And XML
- JavaScript에서 비동기식으로 XML 또는 JSON을 이용해 서버와 데이터를 주고 받을 수 있는 기술
- HTML, CSS, JavaScript, DOM 조작과 XMLHttpRequest object를 활용한 프로그래밍 방식
- AJAX의 대표적인 API ⇒ XMLHttpRequest, Fetch

## XMLHttpRequest

- XHR은 AJAX 요청을 생성하는 JavaScript API
- [XMLHttpRequest](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest) 라는 object를 이용하면 간단하게 서버에게 데이터를 요청하거나 받아올 수 있다

## Fetch API

- [Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)
  는 XMLHttpRequest와 비슷하지만 이보다 좀 더 강력하고 유연한 조작이 가능하다
- 새로 나온지 얼마 되지 않아 인터넷익스플로어에서는 아직 지원이 안된다 (2021.02 기준)

  ![json](/assets/images/javascript/json.png)

# 서버와 데이터를 주고 받는 방법

## XML

- HTML과 같은 마크업 언어(text-based markup language) 중 하나
- HTML과 매우 비슷하지만, HTML처럼 데이터를 보여주는 목적이 아닌 데이터를 저장하고 전달할 목적으로 만들어졌다.
- 서버와 데이터를 주고 받을 때 사용하는 포맷 중 하나

## JSON

- JavaScript Object Notation
- simplest data interchange format 데이터를 주고받을 때 쓸 수 있는 가장 간단한 파일 포맷
- lightweight text-based structure 텍스트를 기반으로 한 가볍고
- easy to read 사람 눈으로 읽기 편하고
- key-value pairs : key와 value로 이루어져 있는 파일 포맷
- used for serialization(직렬화) and transmission of data between the network the network connection : serialization(직렬화)하고 데이터를 전송할 때 사용
- independent programming language and platform 프로그래밍 언어나 플랫폼에 상관없이 사용 가능

✅ Client가 Server에 데이터를 전달할 때나 Server에서 Client에 데이터를 전달할 때 모두 key와 value에 string type으로 변환해 데이터를 전달하여 브라우저에 표기해야 한다. 따라서 object를 serialize해서 string type인 JSON으로 변환하고, JSON을 deserialize해서 object로 다시 변환하는 법을 알아야 한다.

### Object to JSON ([stringify](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify))

- object -serialize→ string(JSON)
- object를 serialize(직렬화)해서 string(JSON)으로 변환할 때 사용

> stringify API

```jsx
stringify(value: any, replacer?: (this: any, key: string, value: any) => any, space?: string | number): string;

Converts a JavaScript value to a JavaScript Object Notation (JSON) string.
@param value A JavaScript value, usually an object or array, to be converted.
@param replacer A function that transforms the results.
@param space Adds indentation, white space, and line break characters to the return-value JSON text to make it easier to read.
```

> stringify 기본 문법

```jsx
JSON.stringify(value[, replacer[, space]])
```

**배열을 JSON으로 변환**

```jsx
json = JSON.stringify(["apple", "banana"]);
console.log(json); // ["apple","banana"]
```

**object를 JSON으로 변환**

- birthDate는 현재 시각이 new Date로 설정되어서 JSON에 포함되어 출력
- jump라는 함수는 JSON에 포함되지 않는다 ⇒ 함수는 object에 있는 데이터가 아니기 때문
- JavaScript에만 존재하는 특별한 데이터도 JSON에 포함되지 않는다 ex. symbol

```jsx
const rabbit = {
  name: "tori",
  color: "white",
  size: null,
  birthDate: new Date(),
  jump: () => {
    console.log(`${this.name} can jump!`);
  },
};

json = JSON.stringify(rabbit);
console.log(json);
// {"name":"tori","color":"white","size":null,"birthDate":"2020-08-22T04:18:16.388Z"}
```

**replacer**

- 해당하는 property만 JSON으로 변환

> replacer에 name을 지정함으로써 name의 value만 출력

```jsx
json = JSON.stringify(rabbit, ["name"]);
console.log(json); // {"name":"tori"}
```

**replacer의 callback함수**

- callback 함수를 통해 좀 더 세밀하게 통제도 가능
- replacer의 callback함수는 key와 value를 전달받음 ⇒ key와 value에 따라 다양한 활용이 가능

> key가 name이라면 dowon을 출력하고, 아니라면 value값을 출력한다는 callback함수를 통해 출력

```jsx
json = JSON.stringify(rabbit, (key, value) => {
  return key === "name" ? "dowon" : value;
});
console.log(json);
// {"name":"dowon","color":"white","size":null,"birthDate":"2020-08-22T04:51:22.845Z"}
```

### JSON to Object ([parse](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse))

- string(JSON) -deserialize→ object
- 직렬화된 JSON을 deserialize해서 object로 다시 변환할 때 사용

> parse API

```jsx
parse(text: string, reviver?: (this: any, key: string, value: any) => any): any;

Converts a JavaScript Object Notation (JSON) string into an object.
@param text A valid JSON string.
@param reviver A function that transforms the results. This function is called for each member of the object.
If a member contains nested objects, the nested objects are transformed before the parent object is.
```

> parse 기본 문법

```jsx
JSON.parse(text[, reviver])
```

**JSON으로부터 object 만들기**

```jsx
const rabbit = {
  name: "tori",
  color: "white",
  size: null,
  birthDate: new Date(),
  jump: () => {
    console.log(`${this.name} can jump!`);
  },
};

json = JSON.stringify(rabbit);
const obj = JSON.parse(json);
console.log(obj);
// {"name":"tori","color":"white","size":null,"birthDate":"2020-08-22T04:51:22.845Z"}
```

**⚠️ object를 JSON으로 변환 후 다시 object로 만들 때 주의할 점**

1. object를 JSON으로 변환할 때 함수는 object의 데이터가 아니므로 변환되지 않는다. (ex. jump)

   ⇒ jump라는 함수가 포함되지 않은 JSON을 다시 object로 변환한다해도 jump라는 함수가 다시 생기지 않는다. JSON으로 변환할 때 이미 없어졌으므로. 따라서 JSON에서 변환된 object에서 기존 함수를 찾게 되면 변환된 obj에는 그 함수가 존재하지 않기 때문에 에러가 발생한다

   > rabbit 안에는 jump라는 함수가 존재하지만, obj에는 존재하지 않는다

   ```jsx
   const rabbit = {
     name: "tori",
     color: "white",
     size: null,
     birthDate: new Date(),
     jump: () => {
       console.log(`${this.name} can jump!`);
     },
   };

   json = JSON.stringify(rabbit);
   console.log(json);
   // {"name":"tori","color":"white","size":null,"birthDate":"2020-08-22T04:18:16.388Z"}

   const obj = JSON.parse(json);
   console.log(obj);
   // {"name":"tori","color":"white","size":null,"birthDate":"2020-08-22T04:51:22.845Z"}

   rabbit.jump(); // can jump!
   obj.jump(); // Uncaught TypeError: obj.jump is not a function at json.js:41
   ```

2. object를 JSON으로 변환하게 되면 string 타입으로 저장된다.

   ⇒ rabbit 안에 birthDate는 object 타입이지만, birthDate가 JSON으로 변환될 때 object에서 string 타입으로 바뀌게 된다. 따라서 JSON에서 다시 object로 변환되어도 한번 바뀐 타입은 돌아오지 않는다.

   > rabbit 안에는 Date라는 object가 존재하지만, obj안에 birthDate는 string이기 때문에 에러가 발생한다

   ```jsx
   console.log(rabbit.birthDate.getDate()); // 22

   console.log(obj.birthDate.getDate());
   // Uncaught TypeError: obj.birthDate.getDate is not a function at json.js:44
   ```

   이때 reviver라는 callback함수를 이용하면 다시 object 타입으로 변환할 수 있다

**reviver**

- reviver가 함수라면, parse로 변환된 결과를 리턴하기 전에 이 인수에 전달해 변형할 수 있다

> reviver를 이용하면 string 타입의 date를 new Date object로 바꾸어서 제대로 출력이 가능하다

```jsx
json = JSON.stringify(rabbit);
console.log(json);
const obj = JSON.parse(json, (key, value) => {
  return key === "birthDate" ? new Date(value) : value;
});
console.log(obj.birthDate.getDate()); // 22
```

# JSON과 관련된 유용한 사이트

- [JSON Diff](http://www.jsondiff.com/)

  서버에서 받아온 첫 번째 데이터와 두 번째 데이터가 어떤 점이 다른지 몰라 비교할 때 사용 (디버깅할 때)

- [JSON Beautifier](https://jsonbeautifier.org/)

  서버에서 받아온 JSON 파일의 포맷이 망가질 때 사용

- [JSON Parser](https://jsonparser.org/)

  JSON 타입을 object 형태로 보고 싶을 때 사용

- [JSON Validator](https://tools.learningcontainer.com/json-validator/)

  JSON이 이상할 때 확인하면 오류를 찾을 수 있다

# Ref.

- [자바스크립트 10. JSON 개념 정리 와 활용방법 및 유용한 사이트 공유 JavaScript JSON - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=FN_D4Ihs3LE&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=10)

- [JSON과 메서드](https://ko.javascript.info/json)

- [JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)

- [코딩교육 티씨피스쿨](http://www.tcpschool.com/json/json_intro_xml)
