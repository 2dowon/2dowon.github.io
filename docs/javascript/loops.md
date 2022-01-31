---
layout: default
title: Loops
last_modified_date: 2021-02-08 16:02:27
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Loops</div>

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

- 원하는 횟수만 반복을 하고 싶다면 for문을 사용, 조건에 맞는 동안 계속 반복하고 싶다면 while문을 사용
- while문 중에서 블럭`{ }`을 먼저 실행하고 싶다면 do-while loop, 조건에 맞는 블럭만 실행하고 싶다면 while loop를 사용
- loop를 완전히 끝내고 싶을 때는 break, 지금 하고 있는 것만 넘어가고 계속 반복을 하고 싶다면 continue

## while loop

while loop는 statement가 false로 나오기 전까지 무한대로 반복한다

```jsx
let i = 3;
while (i > 0) {
  console.log(`while: ${i}`);
  i--;
}
// while:3
// while:2
// while:1
```

## do-while loop

블럭`{ }`을 먼저 실행한 다음에 조건이 맞는지 안 맞는지를 검사한다

```jsx
do {
  console.log(`do while: ${i}`);
  i--;
} while (i > 0);
// do while: 0
```

⇒ i가 위에서 이미 0으로 만들어졌음에도 불구하고 블럭{ }을 먼저 실행하므로 do while:0이 출력되고, 그 다음에 i가 0보다 크지 않다는 것을 확인하고 반복을 멈춘다

## for loop

`for(begin; condition; step)`

⇒ 시작하는 문장, 어떤 조건을 가지고, 어떤 step을 밟아갈 것인지 명시되어 있다

```jsx
for (i = 3; i > 0; i--) {
  console.log(`for: ${i}`);
}
// for: 3
// for: 2
// for: 1
```

> for 안에 let이라는 지역 변수를 설정하는 것도 좋다

```jsx
for (let i = 3; i > 0; i = i - 2) {
  // inline variable declaration
  console.log(`inline variable for: ${i}`);
}
// inline variable for: 3
// inline variable for: 2
// inline variable for: 1
```

## nested loop

for문 안에 for문을 작성할 수 있다 ⇒ nesting중첩해서 사용 가능

하지만 cpu에 좋지 않으므로 최대한 피하는 것이 좋다

```jsx
for (let i = 0; i < 10; i++) {
  for (let j = 0; j < 10; j++) {
    console.log(`i: ${i}, j: ${j}`);
  }
}
```

⇒ i가 0일 때, j를 0부터 9까지 반복하고, 다시 i가 1일 때 j를 0부터 9까지 반복해서 i가 10이 되면 멈춘다

# Ref.

- [자바스크립트 4. 코딩의 기본 operator, if, for loop 코드리뷰 팁 - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=YBjufjBaxHo&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=4)
