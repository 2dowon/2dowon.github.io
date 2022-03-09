---
layout: default
title: JS 논리 연산자 - ‘||=’ 와 ‘??’
last_modified_date: 2022-03-09 20:03:34
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">JS 논리 연산자 - ‘||=’ 와 ‘??’</div>

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

# **`||=` Logical OR assignment**

- 왼쪽 피연산자가 falsy한 값(ex. `0` 또는 `“”` 또는 `null` 또는 `undefined` )일 때 오른쪽 피연산자를 반환하는 논리 연산자
- 단, IE(Internet Explorer) 환경에서는 사용할 수 없다.

```jsx
const a = { duration: 50, title: "" };

a.duration ||= 10;
// a.duration = a.duration || 10;
console.log(a.duration); // 50

a.title ||= "title is empty.";
// a.title = a.title || 'title is empty.';
console.log(a.title); // title is empty.
```

# **`??`** **Nullish coalescing operator**

- 왼쪽 피연산자가 `null` 또는 `undefined` 일때 오른쪽 피연산자를 반환하고, 아니라면 왼쪽 피 연산자를 반환하는 논리 연산자
- `**||**` 논리 연산자는 `null` 또는 `undefined` 뿐만 아니라 falsy한 값(ex. `0` 또는 `“”`)에 해당하는 경우에도 오른쪽 피연산자를 반환한다는 점에서 `??` 논리 연산자와 차이가 있다.
- 단, IE(Internet Explorer) 환경과 Opera Android 환경에서는 사용할 수 없다.

```jsx
const foo = null ?? "default string";
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

# Ref.

- [Logical OR assignment (||=) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment)
- [Nullish coalescing operator - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
