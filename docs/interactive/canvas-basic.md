---
layout: default
title: Canvas
last_modified_date: 2021-03-18 12:03:03
parent: Interactive
---

# Canvas

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

# Canvas 기본 사용법

## HTML - canvas

```jsx
<canvas id="canvas"></canvas>
```

- canvas 태그는 Canvas API 또는 WebGL API을 이용해서 그래픽과 애니메이션을 그릴 때 사용한다
- canvas의 속성에는 width, height만 있다. width와 height를 지정하지 않으면 기본값은 300x150이다.

## JavaScript - rendering contexts

canvas는 처음에 비어있기 때문에 canvas 위에 무언가를 그리기 위해서는 rendering contexts에 접근해야 한다. 따라서 `getContext()` 메서드를 이용해 drawing contexts에 접근할 수 있다.

```jsx
const canvas = document.querySelector("#canvas");
const ctx = canvas.getContext("2d");
```

> 보통 canvas의 크기는 디스플레이 화면의 크기인 clientWidth, clientHeight를 이용해 설정한다. (+) 디스플레이는 가변적이기 때문에 resize될 때마다 디스플레이의 사이즈를 체크해서 캔버스의 크기를 설정해주는 것이 좋다.

```jsx
canvas.width = document.body.clientWidth;
canvas.height = document.body.clientHeight;
```

# canvas로 도형 그리기

canvas로 도형을 그릴 때, 도형은 아래 그림처럼 x, y에서 시작되어서 그려진다.

![https://mdn.mozillademos.org/files/224/Canvas_default_grid.png](https://mdn.mozillademos.org/files/224/Canvas_default_grid.png)

## 사각형 그리기

- [`fillRect(x, y, width, height)`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fillRect) : 색칠된 사각형을 그려준다
- [`strokeRect(x, y, width, height)`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/strokeRect) : 사각형의 외곽 선을 그려준다
- [`clearRect(x, y, width, height)`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/clearRect) : 사각형의 크기만큼 투명하게 만들어준다

## 선 그리기

선을 그릴 때는 먼저 beginPath로 선을 시작한 후, Path methods를 이용해 선을 그린다. 그 후 해당 선에 외곽선만 표시할 것인지, 아니면 선의 영역만큼 색을 채울 것인지 stroke 또는 fill을 이용해 결정한다. 선의 색상은 fillStyle을 이용해 설정할 수 있다.

- [`beginPath()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/beginPath) : 새로운 선을 만든다
- [Path methods](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D#paths) : 선을 그리는 다양한 메서드 ex. moveTo, lineTo 등
- [`closePath()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/closePath) : 선에서 다른 선을 추가할 때 사용
- [`stroke()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/stroke) : 선의 외곽선을 그릴 때 사용
- [`fill()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fill) : 선의 영역만큼 색을 칠할 때 사용

## 원 그리기

원을 그리기 위해서는 [`arc()`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/arc) 메서드를 이용해야 한다. 원은 다른 도형과 다르게 원의 중심이 x, y가 된다.

### arc()

```jsx
arc(x, y, radius, startAngle, endAngle [, clockwise]);
```

- x, y : 원을 그리게 될 가운데 위치
- radius : 반지름 값
- startAngle : angle 시작점
- endAngle : angle 끝나는 점
- clockwise : 옵션값. 시계 또는 반시계 방향을 결정함(기본값 false인 시계 방향)

> Ex. (10, 10) 지점에서 반지름이 5인 원 그리기

```jsx
const canvas = document.querySelector("#canvas");
const ctx = canvas.getContext("2d");
ctx.beginPath();
ctx.arc(10, 10, 5, 0, Math.PI * 2);
ctx.stroke();
```

> 원은 0에서 시작해 2 _ Math.PI에서 끝난다. 따라서 12시 방향부터 그려지는 원을 그리고 싶다면 시작점을 1.5 _ Math.PI 로 설정해야 한다

> (그림 출처 : [https://webisfree.com/2018-06-07/[html5]-캔버스(canvas)에-원-그리기](<https://webisfree.com/2018-06-07/%5Bhtml5%5D-%EC%BA%94%EB%B2%84%EC%8A%A4(canvas)%EC%97%90-%EC%9B%90-%EA%B7%B8%EB%A6%AC%EA%B8%B0>))

![https://webisfree.com/static/uploads/2018/2066_arc_angle.jpg](https://webisfree.com/static/uploads/2018/2066_arc_angle.jpg)

# (+) 사각형, 원, 삼각형, 스마일 그리기

![canvas](/assets/images/interactive/canvas.png)

> index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <canvas id="canvas"></canvas>
    <script src="draw.js"></script>
  </body>
</html>
```

> draw.js

```jsx
const canvas = document.querySelector("#canvas");
const ctx = canvas.getContext("2d");
canvas.width = document.body.clientWidth;
canvas.height = document.body.clientHeight;

function drawRect() {
  ctx.fillStyle = "#02343F";
  ctx.fillRect(10, 10, 150, 100);
}

function drawCircle() {
  ctx.fillStyle = "#F5D042";
  ctx.beginPath();
  ctx.arc(250, 50, 40, 0, Math.PI * 2);
  ctx.fill();
}

function drawTriangle() {
  ctx.fillStyle = "#A4193D";
  ctx.beginPath();
  ctx.moveTo(400, 10);
  ctx.lineTo(360, 90);
  ctx.lineTo(440, 90);
  ctx.fill();
}

function drawSmile() {
  ctx.fillStyle = "#2BAE66";
  ctx.beginPath();
  ctx.arc(575, 55, 50, 0, Math.PI * 2, true); // Outer circle
  ctx.fill();

  ctx.strokeStyle = "#FCF6F5";
  ctx.beginPath();
  ctx.moveTo(610, 55);
  ctx.arc(575, 55, 35, 0, Math.PI, false); // Mouth (clockwise)
  ctx.stroke();

  ctx.fillStyle = "#FCF6F5";
  ctx.beginPath();
  ctx.moveTo(565, 45);
  ctx.arc(560, 45, 5, 0, Math.PI * 2, true); // Left eye
  ctx.fill();

  ctx.fillStyle = "#FCF6F5";
  ctx.beginPath();
  ctx.moveTo(595, 45);
  ctx.arc(590, 45, 5, 0, Math.PI * 2, true); // Right eye
  ctx.fill();
}

drawRect();
drawCircle();
drawTriangle();
drawSmile();
```

# Ref.

- [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)

- [캔버스(Canvas) 기본 사용법](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Basic_usage)

- [Drawing shapes with canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes)

- [[HTML5] 캔버스(Canvas)에 원 그리기](<https://webisfree.com/2018-06-07/%5Bhtml5%5D-%EC%BA%94%EB%B2%84%EC%8A%A4(canvas)%EC%97%90-%EC%9B%90-%EA%B7%B8%EB%A6%AC%EA%B8%B0>)
