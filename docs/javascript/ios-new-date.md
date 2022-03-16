---
layout: default
title: Safari(iOS)에서 new Date 이슈
last_modified_date: 2022-03-16 21:03:34
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Safari(iOS)에서 new Date 이슈</div>

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

# Safari(iOS)에서 new Date 이슈

날짜에 따른 조건을 만들기 위해서 new Date 함수를 이용해 Date 객체를 만들어 사용했다. 하지만 Safari(iOS)에서는 제대로 조건이 동작하지 않는 이슈가 발생했다. (web, Android에서는 정상 동작!)

Safari(iOS)에서만 발생하는 이슈였기에 크로스 브라우징 이슈라 생각해 Safari(iOS) 환경에서 new Date를 실행해본 결과 역시나 제대로 Date 객체가 생성되지 않고 있었다. 이를 해결하기 위한 방법은 생각보다 쉬웠다. Date를 다루는 외부 라이브러리(moment.js 또는 day.js)를 사용하거나 날짜 데이터의 포맷을 잘 맞춰서 넣어주면 된다.

## Chrome

### new Date 결과

- web, Android에서는 아래처럼 문제없이 서버의 날짜 데이터를 Date 객체로 만들 수 있다.

```jsx
const date1 = new Date(); // 현재 날짜 및 시간

const date2 = new Date("2022-3-16");
// Wed Mar 16 2022 00:00:00 GMT+0900 (한국 표준시)

const date3 = new Date("2022-03-16 00:00:00");
// Wed Mar 16 2022 00:00:00 GMT+0900 (한국 표준시)

const date4 = new Date("2022/03/16 00:00:00");
// Wed Mar 16 2022 00:00:00 GMT+0900 (한국 표준시)

const date5 = new Date(2022, 02, 16, 00, 00, 00);
// Wed Mar 16 2022 00:00:00 GMT+0900 (한국 표준시) (월 +1 주의)
```

## Safari

### new Date 이슈 발생

- 크로스 브라우징 이슈로 인해 Safari(iOS)에서는 몇몇 날짜 데이터의 경우 new Date 함수를 이용해 Date 객체를 만들 수 없다는 이슈가 발생했다.
- 아래에서 보는 것처럼 date2와 date3는 Safari(iOS)에서 Date 객체를 만들 수 없었다.

```jsx
const date1 = new Date(); // 현재 날짜 및 시간

const date2 = new Date("2022-3-16");
// Invalid Date

const date3 = new Date("2022-03-16 00:00:00");
// Invalid Date
```

### new Date 해결

- 제일 간단한 방법으로 date4나 date5처럼 날짜의 형식을 `“YYYY-MM-DD HH-MM-SS"` 또는 `YYYY,MM,DD,HH,MM,SS` 이렇게 맞춰주면 된다.
- 또는 moment.js나 day.js 라이브러리를 이용해서도 해결할 수 있다.

```jsx
const date4 = new Date("2022/03/16 00:00:00");
// Wed Mar 16 2022 00:00:00 GMT+0900 (한국 표준시)

const date5 = new Date(2022, 02, 16, 00, 00, 00);
// Wed Mar 16 2022 00:00:00 GMT+0900 (한국 표준시) (월 +1 주의)
```

# Ref.

- [아이폰(IOS) javascript new Date 이슈](https://gosasac.tistory.com/48)

- [Safari에서 new Date()는 다르다? · Issue #69 · SeonHyungJo/Tip-Note](https://github.com/SeonHyungJo/Tip-Note/issues/69)
