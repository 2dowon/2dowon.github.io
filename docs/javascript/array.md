---
layout: default
title: Array
last_modified_date: 2021-02-10 21:02:43
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Array</div>

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

# Array 배열

- 칸칸이 모여 있는 자료구조로 index가 지정되어 있다 (index는 0부터 시작)
- 한 배열 안에는 동일한 type의 자료를 넣는 것이 중요하다

  (다른 언어와 다르게 JavaScript에서는 여러 타입의 자료를 넣을 수 있지만, 이는 좋지 않은 방법이다)

## Declaration

array를 선언하는 방법

```jsx
const arr1 = new Array();
```

```jsx
const arr2 = [1, 2];
```

## Index position

index를 기준으로 데이터가 설정되기 때문에 index를 통해서 데이터에 접근할 수 있다

```jsx
const fruits = ["🍎", "🍌"];
console.log(fruits); // ["🍎", "🍌"]
console.log(fruits.length); // 2
```

- index를 제공하면 index에 해당하는 value를 받아올 수 있다
- 배열의 첫 번째 value를 찾을 때는 0을 사용, 마지막 value를 찾을 때는 `length-1`을 사용

```jsx
console.log(fruits[0]); // 🍎
console.log(fruits[1]); // 🍌
console.log(fruits[fruits.length - 1]); // 🍌
```

## Looping over an array

fruits 배열에 들어있는 모든 과일을 출력하기 위해 for, for of, forEach 모두 사용 가능하다.

### for

```jsx
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]); // 🍎 🍌
}
```

### for of

```jsx
for (let fruit of fruits) {
  console.log(fruit); // 🍎 🍌
}
```

### forEach

```jsx
forEach(callbackfn: (value: T, index: number, array: T[]) => void, thisArg?: any): void;
```

- 총 2개의 parameter를 전달 ⇒ callbackfn, thisArg
- 전달한 callbackfn(콜백함수)를 value마다 호출해주고, callbackfn에는 총 3가지의 인자가 들어온다. (value, index, array)

> forEach로 fruit, index, array 출력 (index, array는 보통 잘 출력하지 않는다)

```jsx
fruits.forEach(function (fruit, index, array) {
  console.log(fruit, index, array);
});
// 🍎  0   ["🍎", "🍌"]
// 🍌  1   ["🍎", "🍌"]
```

> 이름이 없는 annoymous함수의 경우 arrow를 사용할 수 있다

```jsx
fruits.forEach((fruit) => console.log(fruit));
// 🍎 🍌
```

## Addition, deletion, copy

### push

add an item to the end ⇒ 맨 뒤에 추가

```jsx
const fruits = ["🍎", "🍌"];
fruits.push("🍓", "🍑");
console.log(fruits);
// ["🍎", "🍌", "🍓", "🍑"]
```

### pop

remove an item to the end ⇒ 맨 뒤에서부터 제거

```jsx
const fruits = ["🍎", "🍌", "🍓", "🍑"];
fruits.pop();
fruits.pop();
console.log(fruits);
// ["🍎", "🍌"]
```

### unshift

add an item to the beginning ⇒ 맨 앞에서부터 추가

```jsx
const fruits = ["🍎", "🍌"];
fruits.unshift("🍓", "🍋");
console.log(fruits);
// ["🍓", "🍋", "🍎", "🍌"]
```

### shift

remove an item to the beginning ⇒ 맨 앞에서부터 제거

(+) unshift, shift는 pop, push에 비해서 매우 느리다 (특히 배열의 길이가 길수록 ⇒ 시간복잡도 $O(N)$) 앞에 추가하거나 빼는 것이기 때문에 모든 데이터들을 옮겨야 하기 때문이다

```jsx
const fruits = ["🍓", "🍋", "🍎", "🍌"];
fruits.shift();
fruits.shift();
console.log(fruits); // ["🍎", "🍌"]
```

### splice

remove an item by index position ⇒ 지정된 위치의 item을 제거하는 것

> `splice(지우기 시작하고 싶은 index의 위치, 지우고 싶은 개수)`

```jsx
const fruits = ["🍎", "🍌", "🍓", "🍑", "🍋"];
fruits.splice(1, 1);
console.log(fruits); // ["🍎", "🍓", "🍑", "🍋"]
```

> 지우고 싶은 개수를 적지 않는다면, 지우고 싶은 위치부터 끝까지 다 지워버린다

```jsx
const fruits = ["🍎", "🍓", "🍑", "🍋"];
fruits.splice(1);
console.log(fruits); // ["🍎"]
```

> splice 후에 그 자리에 추가하는 것도 가능

```jsx
const fruits = ["🍎", "🍓", "🍑", "🍋"];
fruits.splice(1, 1, "🍏", "🍉");
console.log(fruits); // ["🍎", "🍏", "🍉", "🍑", "🍋"]
```

### concat

두 가지의 배열을 묶어서도 만들 수 있다

```jsx
const fruits = ["🍎", "🍏", "🍉", "🍑", "🍋"];
const fruits2 = ["🍐", "🥝"];
const newFruits = fruits.concat(fruits2);
console.log(newFruits); // ["🍎", "🍏", "🍉", "🍑", "🍋", "🍐", "🥝"]
```

## Searching

### indexOf

index가 몇 번째에 위치하고 있는지 알고 싶을 때 사용한다. 배열 안에 해당하는 값이 없으면 -1이 출력된다.

```jsx
const fruits = ["🍎", "🍏", "🍉", "🍑", "🍋", "🍐", "🥝"];
console.log(fruits.indexOf("🍎")); // 0
console.log(fruits.indexOf("🍉")); // 2
console.log(fruits.indexOf("⛄️")); // -1
```

### includes

특정 index가 포함되어 있는지를 알고 싶을 때 true/false로 결과를 리턴한다.

```jsx
const fruits = ["🍎", "🍏", "🍉", "🍑", "🍋", "🍐", "🥝"];
console.log(fruits.includes("🍎")); // true
console.log(fruits.includes("⛄️")); // false
```

### lastIndexOf

indexOf는 똑같은 값이 여러개 있으면 가장 앞에 있는 값의 위치를 알려준다

> `indexOf` ⇒ 사과가 index 0, 5번에 둘 다 있지만 가장 앞에 있는 값인 0이 리턴된다

```jsx
const fruits = ["🍎", "🍏", "🍉", "🍑", "🍋", "🍎"];
console.log(fruits.indexOf("🍎")); // 0
```

> `lastIndexOf` ⇒ 똑같은 값이 여러개 있으면 가장 마지막에 있는 값의 위치를 알려준다

```jsx
console.log(fruits.lastIndexOf("🍎")); // 5
```

## (+) 유용한 배열 함수들

### [join](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

- 배열의 모든 item을 string으로 나타내고 싶을 때 사용
- join 함수의 기본 문법

  ```jsx
  arr.join([separator]);
  ```

> 배열 fruits의 모든 item을 string으로 변환해 출력

```jsx
const fruits = ["apple", "banana", "orange"];
const result = fruits.**join**(",");
console.log(result); // apple,banana,orange
```

### [split](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)

- string을 array로 변환하고 싶을 때 사용
- split 함수의 기본 문법

  ```jsx
  str.split([separator[, limit]])
  ```

  - separator(구분자)는 꼭 전달해야 된다! 구분자를 전달하지 않으면 하나의 string이 되어버리기 때문에
  - limit은 전달하고 싶은 index의 수를 말한다. ex. limit 자리에 2를 넣으면 2개의 index만 출력된다.

> string인 fruits를 ,로 구분해서 배열 만들기

```jsx
const fruits = "🍎, 🥝, 🍌, 🍒";
const result = fruits.split(",");
console.log(result); // ["🍎", " 🥝", " 🍌", " 🍒"]
```

### [reverse](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

- 주어진 배열의 순서를 거꾸로 만들 때 사용
- 이때 reverse는 array의 배열 자체를 거꾸로 만들어주고, 리턴값도 거꾸로 되어 그 값을 리턴하는 것
- reverse 함수의 기본 문법

  ```jsx
  a.reverse();
  ```

> 배열 array의 순서를 거꾸로 만들기

```jsx
const array = [1, 2, 3, 4, 5];
const result = array.reverse();
console.log(result); // [5, 4, 3, 2, 1]
console.log(array); // [5, 4, 3, 2, 1]
```

### [slice](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

- 배열의 일부를 slice해서 새로운 배열을 만들 때 사용
- slice 함수의 기본 문법

  ```jsx
  arr.slice([begin[, end]])
  ```

  - begin : 받아오고자 하는 index의 순서
  - end : 어디까지 받아오는지에 대한 순서인데, 이 때 end의 숫자는 배제된다 (ex. end=3이면 3의 index는 제외하고 2까지만 받아오게 됨)

> 배열 array 안에서 첫 번째와 두 번째 item을 제거하고 새로운 배열 result 만들기

```jsx
const array = [1, 2, 3, 4, 5];
const result = array.slice(2, 5);
console.log(result); // [3, 4, 5]
console.log(array); // [1, 2, 3, 4, 5]
```

### [splice](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

- 배열의 일부를 삭제 또는 교체해서 기존의 배열을 수정할 때 사용
- splice 함수의 기본 문법

  ```jsx
  array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
  ```

> 배열 array의 첫 번째와 두 번째 item만 남기고, 3, 4, 5로 구성된 새로운 배열 만들기

```jsx
const array = [1, 2, 3, 4, 5];
const result = array.splice(2, 5);
console.log(result); // [3, 4, 5]
console.log(array); // [1, 2]
```

### [find](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

- 주어진 조건을 만족하는 첫 번째 요소의 값을 리턴한다
- 해당 조건을 만족하는 값이 없다면 undefined를 리턴한다
- find 함수의 기본 문법

  ```jsx
  arr.find(callback[, thisArg])
  ```

  ⇒ callback함수를 만들어서 그 함수가 true일 때 찾아지는 첫 번째 element의 값을 Boolean type으로 리턴한다

> find a student with the score 90

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students.find(function (student) {
  return student.score === 90;
});
console.log(result); // Student {name: "C", age: 30, enrolled: true, score: 90}
```

```jsx
const result = students.find((student) => student.score === 90);
console.log(result); // Student {name: "C", age: 30, enrolled: true, score: 90}
```

### [filter](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

- 필터를 이용하면 우리가 원하는 값만 받아서 새로운 배열로 반환할 수 있다
- filter의 기본 문법

  ```jsx
  arr.filter(callback(element[, index[, array]])[, thisArg])
  ```

> make an array of enrolled students

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students.filter((student) => student.enrolled === true);
console.log(result);
```

⇒ [Student, Student, Student]

- 0: Student {name: "A", age: 29, enrolled: true, score: 45}
- 1: Student {name: "C", age: 30, enrolled: true, score: 90}
- 2: Student {name: "E", age: 18, enrolled: true, score: 88}

### [map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

- 배열 안의 각각의 요소들이 callback함수를 거쳐서 다시 새로운 값으로 변환하는 것
- 배열 안에 있는 요소들을 함수를 통해 다른 방식으로 가져오고 싶을 때 사용
- map 함수의 기본 문법

  ```jsx
  arr.map(callback(currentValue[, index[, array]])[, thisArg])
  ```

  - callback함수에 따라 각각의 요소들이 다른 값으로 mapping 된다
  - callback 함수의 이름은 최대한 한번에 이해하기 쉽게 쓰는 것이 좋다

> make an array containing only the students' scores

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students.map((student) => student.score);
console.log(result); // [45, 80, 90, 66, 88]
```

### [some](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/some)

- 배열의 요소 중에서 callback함수의 값이 true가 있는지 없는지를 체크해서 true/false로 알려준다
- 요소 중 하나라도 조건에 맞으면 true가 리턴된다
- 빈 배열에서 호출하면 무조건 false를 리턴한다
- some 함수의 기본 문법

  ```jsx
  arr.some(callback[, thisArg])
  ```

> check if there is a student with the score lower than 50

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students.some((student) => student.score < 50);
console.log(result); // true
```

### [every](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

- 배열에 들어있는 모든 요소들이 callback 함수의 조건을 충족해야지만 true가 리턴된다
- every 함수의 기본 문법

  ```jsx
  arr.every(callback[, thisArg])
  ```

> check if there is a student with the score lower than 50

⇒ 모든 학생들의 점수가 50점 이상이 아니므로 false인데, `!`를 씀으로써 반대인 true로 출력한다

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result2 = **!**students.every((student) => student.score >= 50);
console.log(result2); // true
```

### [reduce](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

- 우리가 원하는 시작점부터 모든 배열을 돌면서 어떤 값을 누적할 때 사용
- reduce 함수의 기본 문법

  ```jsx
  arr.reduce((prev, curr)
  ```

  - prev : 리턴된 값이 그 다음으로 호출될 때 prev로 연결된다 (리턴하는 값들이 prev로 순차적으로 전달된다)
  - curr : 배열 하나하나씩 순차적으로 curr에 전달이 된다

> 모든 학생들의 점수 합

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students.reduce((prev, curr) => {
  return prev + curr.score;
}, **0**); // 여기서 0은 처음 시작을 0에서부터 시작한다는 뜻
console.log(result); // 369
```

```jsx
const result = students.reduce((prev, curr) => prev + curr.score, 0);
console.log(result); // 369
```

### [reduceRight](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/ReduceRight)

- reduce와 같지만, 배열이 제일 뒤에서부터 시작하고 싶을 때 사용

> 모든 학생들의 점수 합

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students.reduceRight((prev, curr) => prev + curr.score, 0);
console.log(result); // 369
```

> 학생들의 점수 평균

⇒ reduce를 이용해서 합계를 구한 후 개수만큼 나눠주면 평균값을 구할 수 있음

```jsx
const result = students.reduce((prev, curr) => prev + curr.score, 0);
console.log(result / students.length); // 73.8
```

### [sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

- 배열을 일정 순서에 따라 정렬할 때 사용
- 오름차순 (낮은 숫자부터) : `sort((a, b) => a - b)`
- 내림차순 (높은 숫자부터) : `sort((a, b) => b - a)`

> 학생들의 성적을 오름차순으로 정렬

```jsx
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}
const students = [
  new Student("A", 29, true, 45),
  new Student("B", 28, false, 80),
  new Student("C", 30, true, 90),
  new Student("D", 40, false, 66),
  new Student("E", 18, true, 88),
];

const result = students
  .map((student) => student.score)
  .sort((a, b) => a - b)
  .join();
console.log(result); // 45, 66, 80, 88, 90
```

# Ref.

- [자바스크립트 8. 배열 제대로 알고 쓰자. 자바스크립트 배열 개념과 APIs 총정리 - 프론트엔드 개발자 입문편 (JavaScript ES6 )](https://www.youtube.com/watch?v=yOdAVDuHUKQ&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=8)

- [자바스크립트 9. 유용한 10가지 배열 함수들. Array APIs 총정리 - 프론트엔드 개발자 입문편 ( JavaScript ES6)](https://www.youtube.com/watch?v=3CUjtKJ7PJg&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=9)
