---
layout: default
title: 마우스 따라다니는 원 만들기
last_modified_date: 2021-02-15 10:02:26
parent: JavaScript
---

# JS 자료구조 - ES6 Map

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

![mouse_animation](/assets/images/javascript/mouse_animation.gif)

브라우저의 좌표 개념을 복습하기 위해 JavaScript를 이용해서 마우스를 따라다니는 원을 만들었다. 사실 처음에는 어떻게 만들지 했는데, 막상 만들어보니까 생각보다 간단해서 정리해두려고 한다.

너무 간단한 코드라 CSS와 JavaScript 코드를 따로 분리하지 않고 HTML에 같이 적어서 완성했다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mouse Animation</title>
    <style>
      * {
        margin: 0;
      }
      body {
        background-color: #3d5a80;
      }
      .circle {
        position: absolute;
        top: 0;
        left: 0;
        width: 150px;
        height: 150px;
        border-radius: 50%;
        background-color: #ee6c4d;
        transform: translate(-50%, -50%);
      }
    </style>
  </head>
  <body>
    <div class="circle"></div>
    <script>
      const circle = document.querySelector(".circle");

      document.addEventListener("mousemove", (e) => {
        const mouseX = e.clientX;
        const mouseY = e.clientY;
        circle.style.left = mouseX + "px";
        circle.style.top = mouseY + "px";
      });
    </script>
  </body>
</html>
```

## 1. 마우스 좌표

마우스가 원을 따라다니도록 만들기 위해서는 일단 마우스의 좌표를 알아야 한다. 마우스의 좌표는 `clientX` 와 `clientY` 를 이용해 알 수 있다. clientX, clientY는 브라우저 window의 좌표값 위치를 전달하기 때문이다. 페이지에서 떨어져있는 좌표값(스크롤된 길이를 포함) 위치를 전달하는 pageX, pageY와는 다르다.

## 2. mousemove

마우스의 좌표를 알았다면 이제 마우스가 움직일 때마다 마우스의 좌표값을 알아야하므로 mousemove 이벤트를 이용해야 한다.

> 마우스가 움직일 때마다 console에 마우스의 좌표값을 출력한다.

```java
document.addEventListener("mousemove", (e) => {
    console.log(e.clientX);
    console.log(e.clientY);
});
```

## 3. 원에 마우스의 좌표값 적용하기

원이 마우스를 따라다니기 위해서는 원도 마우스와 같은 좌표값을 가지고 있어야 한다. clientX는 css의 left와 동일한 값이고, clientY는 css의 top과 동일한 값이다. 따라서 clientX, clientY를 이용해 구한 마우스의 좌표값을 원의 top, left에 적용해주면 된다.

```java
document.addEventListener("mousemove", (e) => {
    const mouseX = e.clientX;
    const mouseY = e.clientY;
    circle.style.left = mouseX + 'px';
    circle.style.top = mouseY + 'px';
});
```

하지만 만약 원이 top, left 값을 기존에 가지고 있지 않더라면 JavaScript에서 설정한 값이 적용되지 않으므로 반드시 원의 css에서 top, left 값을 초기값인 0px로 설정해줘야 한다. 그리고 모든 요소의 position의 기본 값은 static이기 때문에 top, left 값이 적용되지 않아서 position을 absolute로 설정했다. 마지막으로 top, left 값은 요소의 가운데가 아닌 left와 top을 기준으로 적용되기 때문에 정가운데 위치하기 위해서는 X축과 Y축을 -50%만큼 이동해줘야 한다.

```css
.circle {
  position: absolute;
  top: 0;
  left: 0;
  width: 150px;
  height: 150px;
  border-radius: 50%;
  background-color: #ee6c4d;
  transform: translate(-50%, -50%);
}
```

# Ref.

- [[자바스크립트]마우스 따라다니는 박스만들기](https://andwinter.tistory.com/284)

- [Element: mousemove event](https://developer.mozilla.org/en-US/docs/Web/API/Element/mousemove_event)

- [MouseEvent.clientX](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/clientX)
