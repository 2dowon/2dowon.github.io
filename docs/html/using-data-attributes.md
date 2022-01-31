---
layout: default
title: 데이터 속성 사용하기
last_modified_date: 2021-03-03 21:03:81
parent: HTML
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">데이터 속성 사용하기</div>

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

## HTML 문법

- HTML에서 데이터 속성을 이용하면 HTML에도 데이터를 저장할 수 있다. 이는 HTML5가 도입되면서 추가된 문법이다.
- 데이터 속성을 이용하기 위해서는 데이터를 저장하고자 하는 element `data-` 로 시작하는 속성을 사용하면 된다.
- 데이터 속성은 화면에는 보이지 않게 글이나 추가 정보를 담을 수 있게 해준다.

> li에 data-type, data-lang 정보를 추가했다.

```html
<li class="item" data-type="book" data-lang="html" data-index-number="123">
  ...
</li>
```

## JavaScript 에서 접근하기

- HTML에서 데이터 속성을 이용해 저장한 데이터는 JavaScript로 가져올 수 있다. JavaScript로 가져오기 위해서는 dataset 객체를 통해 data 속성 이름의 `data-` 뒷 부분을 사용하면 된다.
- 데이터 속성의 이름은 아래 규칙에 따라 DOMStringMap의 key로 변환된다.
  - 접두사 `data-` 는 삭제된다.
  - a부터 z까지 소문자 앞에 오는 모든 대시는 삭제되고, 해당 소문자는 대문자로 변환된다
  - ex. data-index-number → indexNumber
- 데이터를 읽을 때는 `element.dataset.keyname` 또는 `element.dataset[keyname]` 을 이용해 읽을 수 있다.
- JavaScript에서 데이터 속성 값을 읽는 것 뿐만 아니라 다른 값으로 수정할 수도 있다.

```jsx
const item = document.querySelector(".item");

// 데이터 속성 값 가져오기
console.log(item.dataset.type); // book
console.log(item.dataset["lang"]); // html
console.log(item.dataset.indexNumber); // 123

// 데이터 속성 값 수정하기
item.dataset.lang = "javascript";
console.log(item.dataset.lang); // javascript
```

## CSS에서 접근하기

- 데이터 속성은 순 HTML 속성이기 때문에 CSS에서도 접근할 수 있다.
- 데이터 값은 문자열이다. 따라서 CSS에서 접근할 때 데이터 값이 숫자인 경우에는 따옴표 안에 써줘야 한다.

> data-type이 book인 li의 background-color를 검은색으로 하기

```css
li[data-type="book"] {
  background-color: black;
}

li[data-index-number="123"] {
  color: white;
}
```

### ⚠️ 주의사항

데이터 속성을 이용해 데이터를 저장하는 방법은 결국 HTML에 데이터를 넣는 것이기 때문에 민감한 데이터를 넣지 않도록 주의해야 한다. HTML은 누구나 볼 수 있고, JavaScript로 접근 가능하기 때문에 누구나 수정할 수 있다. 또한 데이터 속성 값은 검색 크롤러가 찾지 못하기 때문에 검색에 노출되어야 하는 데이터는 이 점도 유의해야 한다.

# Ref.

- [데이터 속성 사용하기](https://developer.mozilla.org/ko/docs/Learn/HTML/Howto/Use_data_attributes)

- [HTMLElement.dataset](https://developer.mozilla.org/ko/docs/Web/API/HTMLOrForeignElement/dataset)

- [(HTML&DOM) HTML에 데이터 저장하기 - data-속성과 dataset](https://www.zerocho.com/category/HTML&DOM/post/5a76d1eaabd090001b981ba6)
