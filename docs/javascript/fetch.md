---
layout: default
title: Fetch API
last_modified_date: 2021-03-28 20:03:87
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Fetch API</div>

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

JavaScript에서는 필요할 때 서버에 네트워크 요청을 보내고 새로운 정보를 받아오는 일을 할 수 있다. AJAX를 이용해서도 할 수 있지만, fetch 메서드를 이용하면 더 간편하게 할 수 있다. 따라서 fetch를 이용하면 JSON 데이터를 동적으로 가져올 수 있다.

⚠️ 단, fetch는 최신 API로 모든 브라우저에서 다 지원되는 것은 아니다.

## fetch 기본 문법

```jsx
fetch(url, [options]);
```

- url : 접근하고자 하는 URL
- options : 선택 매개변수, method나 header 등을 지정할 수 있다. 아무것도 넘기지 않으면 요청은 GET 방식으로 진행된다.

## GET 방식

```jsx
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  [.catch(err => console.error(err))];
```

- fetch를 호출하면 브라우저는 네트워크 요청을 보내고 서버로부터 응답 헤더를 받는다. 이 때 response가 200(성공)이면 데이터를 받아올 수 있다는 뜻이다.
- response를 JSON 형태로 파싱한 다음, 이를 콘솔 창에서 확인할 수 있다
- 마지막으로 catch를 통해 요청에 대한 에러를 확인할 수 있다.

### async & await를 이용하기

- async & await를 이용하면 then을 이용하지 않아도 fetch를 사용할 수 있다.
- 간단한 구현은 promise를 이용하는 것이 더 깔끔할 수 있지만, 만약 복잡해질 경우 promise chaining은 가독성이 좋지 않아 async와 await를 이용하여 fetch를 사용하는 것이 더 좋을 수 있다.

```jsx
async function getData() {
  const response = await fetch(url);
  const json = await response.json();
  console.log(json);
}
```

- `try...catch` 를 이용해 에러를 핸들링할 수 있다

```jsx
async function getData() {
  try {
    const response = await fetch(유효하지 않은 url);
    const json = await response.json();
    console.log(json);
    } catch(err) {
        alert(err);
    }
}
```

## POST 방식

- 어떠한 정보를 백엔드로 전송하기 위해서는 POST를 사용해야 한다.
- GET 이외의 요청을 보내려면 옵션을 사용해야 하므로, POST 요청을 보내기 위해서는 `method : "POST"` 로 지정하면 된다.
- 옵션 headers를 통해 JSON 포맷을 사용한다고 알려줘야 한다
- 요청 전문은 stringify() 메서드를 이용해 직렬화해서 body 본문에 실어 보낸다.

```jsx
const user = {
  name: "dowon",
  userId: 1,
};

fetch(url, {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(user),
})
  .then((response) => response.json())
  .then((data) => console.log(data));
// {name: "dowon", userId: 1, id: 101}
```

## API 명세를 보고 fetch 사용하기

### GET

> API 명세

```jsx
//설명: 유저 정보를 가져온다.
base url: https://api.google.com
endpoint: /user/3
method: GET
응답형태:
    {
        "success": boolean,
        "user": {
            "name": string,
            "batch": number
        }
    }
```

> 구현

```jsx
fetch('https://api.google.com/user/3')
  .then(res => res.json())
  .then(res => {
    if (res.success) {
        console.log(`${res.user.name}` 님 환영합니다);
    }
});
```

### POST

> API 명세

```jsx
설명: 유저를 저장한다.
base url: https://api.google.com
endpoint: /user
method: post
요청 body:
    {
        "name": string,
        "batch": number
    }

응답 body:
    {
        "success": boolean
    }
```

> 구현

```jsx
fetch("https://api.google.com/user", {
  method: "post",
  body: JSON.stringify({
    name: "dowon",
    batch: 1,
  }),
})
  .then((res) => res.json())
  .then((res) => {
    if (res.success) {
      alert("저장 완료");
    }
  });
```

# Ref.

- [Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

- [fetch](https://ko.javascript.info/fetch)

- [[자바스크립트] fetch() 함수로 API 호출하기](https://www.daleseo.com/js-window-fetch/)

- [fetch() 함수 사용법](https://yeri-kim.github.io/posts/fetch/)

- [[JS] 자바스크립트 fetch API 사용하기](https://hogni.tistory.com/142)
