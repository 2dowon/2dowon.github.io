---
layout: default
title: react-intersection-observer로 infinite scroll 만들기
last_modified_date: 2022-05-21 22:12:35
parent: React
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">react-intersection-observer로 infinite scroll 만들기</div>

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

# \***\*IntersectionObserver\*\***

![intersection-observer](/assets/images/react/intersection-observer.png)

`[IntersectionObserver` API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API)는 위 이미지처럼 Target element가 Viewport 안에 보여지고 있는지를 관찰하는 API이다. Image Lasy loading, Infinitie Scroll 등을 구현할 때 활용할 수 있는 API로 직접 사용해도 되지만, react-intersection-observer와 같은 라이브러리를 이용한다면 더 쉽게 사용할 수 있다.

# react-intersection-observer

react-intersection-observer 사용은 생각보다 매우 간단하다.

### install

- `yarn add react-intersection-observer`
- `npm install react-intersection-observer --save`

### example

그 다음으로는 react-intersection-observer 라이브러리 설명에 나와있는 아래 코드처럼 브라우저의 viewport에 보여질 때 체크할 element에 `ref` 속성을 준다. 그 후에는 브라우저의 viewport에 ref로 연결된 element가 보인다면 `inView` 값은 true가 된다.

```jsx
import React from "react";
import { useInView } from "react-intersection-observer";

const Component = () => {
  const { ref, inView, entry } = useInView({
    /* Optional options */
    threshold: 0,
  });

  return (
    <div ref={ref}>
      <h2>{`Header inside viewport ${inView}.`}</h2>
    </div>
  );
};
```

### useInView의 props

- `root` : target element를 관찰하는 viewport로 사용될 element로 기본값은 브라우저의 viewport이다.
- `rootMargin` : root의 margin을 이용해 root 범위를 확장하거나 축소할 수 있다. px이나 %로 줄 수 있고, 기본값은 0이다.
- `threshold` : target element가 root에 얼마나 보였을 때 노출되었다고 판단하는지 결정하는 값이다. 0.0부터 1.0까지 지정 가능하고, 0.0은 target element가 1px이라도 보이면, 1.0은 요소가 전부 보일때 노출됬다고 판단한다.
- `onChange` : inview의 상태가 변화될 때마다 실행되는 콜백함수이다.
- `skip`: IntersectionObserver를 생성하는 것을 스킵할 수 있다. 기본값은 false이다.
- `triggerOnce` : observer가 딱 한번만 실행된다. 기본값은 false이다.
- `initialInView` : inView의 초깃값을 설정할 수 있다. 만약 처음부터 element가 보일 것으로 예상된다면 해당 속성에 true를 주면 된다. 기본값은 false이다.
- `fallbackInView` : 만약 IntersectionObserver API가 실행되지 않고 에러가 발생할 경우, inView 값을 설정할 수 있다. 기본값은 false이다.
- 🧪 trackVisibility, delay는 아직 실험적인 props이다.

# **infinite scroll (무한 스크롤) 구현하기**

infinite scroll을 구현하는 방법은 다양하다. 전에는 스크롤이 될 때마다 남은 스크롤 길이를 파악해서 스크롤 길이가 일정 길이 미만으로 남았을 때 데이터를 다시 가져오도록 만들었었는데, 이번에는 좀 다른 방식으로 구현해봤다. Intersection Observer API를 이용해서 하는 방식인데, [react-intersection-observer 라이브러리](https://www.npmjs.com/package/react-intersection-observer)를 이용했다.

### 그 전에 왜 굳이 Intersection Observer를 이용해 무한 스크롤을 구현할까 🤔

위에 말했듯이 infinite scroll을 구현하는 방법은 다양하다. 대표적인 예로, Scroll Event를 감지해서 스크롤의 남은 길이를 통해 일정 길이 미만이 남을 경우에 다시 데이터를 가져오도록 하는 방식이 있다.

하지만 이 방식의 경우, 너무 많은 Scroll Event를 감지함에 따라 `debounce` & `throttle` 를 사용해서 성능을 개선할 필요가 있다. 또한 스크롤에 따라 스크롤 길이를 구하기 위해 `offset` 값을 구하는데, 이 때 정확한 값을 구하기 위해서 매번 layout을 새로 그려야한다. 즉, Reflow가 발생하기 때문에 성능이 저하된다. 이와 같은 이유를 봤을 때, Intersection Observer를 이용해 무한 스크롤을 구현하는 것이 성능적으로 낫다.

## react-intersection-observer로 무한 스크롤 하기

```jsx
import React, { useEffect } from "react";
import { useInView } from "react-intersection-observer";

const Component = () => {
  const [ref, inView] = useInView();

  const fetchNextPage = () => {
    // 다음 데이터 가져오기
    // useInfiniteQuery를 사용한다면 해당 함수를 사용할 수 있습니다!
  };

  useEffect(() => {
    if (!inView) {
      return;
    }
    fetchNextPage();
  }, [inView]);

  return (
    <div>
      <div>데이터를 보여줍시다!</div>
      <div ref={ref}></div>
    </div>
  );
};
```

1. 데이터를 끝까지 다 봤는지, 즉 스크롤을 맨 밑까지 다 했는지 확인하기 위해서 먼저 데이터를 가져오는 element 바로 아래에 빈 div를 하나 만들어준다.
2. 빈 div에 react-intersection-observer의 `ref`를 연결해주면, 이제 스크롤을 맨 밑으로 내리면 빈 div가 viewport 안에 들어오면서 `inView` 값이 true가 된다.
3. useEffect를 통해 inView값이 변화될 때, 다음 데이터를 가져오는 함수 `fetchNextPage`가 실행된다.
4. 다음 데이터를 가져오면 스크롤이 생기면서 `ref`가 연결된 div가 다시 viewport 밖으로 벗어나면서 `inView`값은 false가 된다. 이 경우에는 inView 값이 true가 아니기 때문에 `fetchNextPage`가 실행되지 않는다.

# Ref.

- [화면에 보이는 요소를 감지하는 IntersectionObserver 알아보기](https://cross-code.github.io/posts/IntersectionObserver/)

- [react-intersection-observer](https://www.npmjs.com/package/react-intersection-observer)

- [React 무한 스크롤 구현하기 with Intersection Observer](https://velog.io/@jce1407/React-%EB%AC%B4%ED%95%9C-%EC%8A%A4%ED%81%AC%EB%A1%A4-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-with-Intersection-Observer)
