---
layout: default
title: paintJS
last_modified_date: 2021-03-05 22:03:80
parent: Projects
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">paintJS</div>

## Introduction

- [바닐라 JS로 그림판 만들기](https://nomadcoders.co/javascript-for-beginners-2) 강의를 바탕으로 간단한 그림판을 만들었습니다.
- 강의 내용의 기본 기능 외에 추가 기능을 적용했습니다.
- 해당 프로젝트가 궁금하다면 [PaintJS🎨](https://2dowon.github.io/NomadCoders_paintJS/) 를 클릭해주세요!
- 해당 프로젝트 소스 코드는 [GitHub](https://github.com/2dowon/NomadCoders_paintJS)에서 확인할 수 있습니다.

## Period

2021.03.05

## Stack

- HTML
- CSS
- JavaScript

## paintJS - 기본 기능

- 원하는 색을 선택한 후에 그림을 그리거나 배경을 칠할 수 있습니다.
- PAINT(기본값) 버튼을 누르면 그림을 그릴 수 있습니다.
- FILL 버튼을 누르면 배경을 칠할 수 있습니다.
- 슬라이드 바를 조정해 브러쉬의 두께를 조정할 수 있습니다.
- SAVE 버튼을 누르면 png 파일로 이미지를 저장할 수 있습니다.
  ![basic](/assets/images/project/basic.gif)

## paintJS - 추가 기능

### 이미지 업로드

- IMAGE 버튼을 눌러서 원하는 이미지를 그림판에 업로드할 수 있습니다.
- 업로드한 이미지 위에 그림을 그린 후 저장할 수 있습니다.
  ![upload_image](/assets/images/project/upload_image.gif)

### 더 많은 색상 추가

- 기본으로 제공되는 색상 외에 원하는 색을 선택할 수 있습니다.

### 지우기 기능

- CLEAR 버튼을 누르면 지금까지 그린 모든 작업을 한 번에 다 지울 수 있습니다.
  ![color_and_clear](/assets/images/project/color_and_clear.gif)

# 회고

- 단순히 강의를 보고 따라하는 것이 아닌 내가 더 추가하고 싶다고 생각한 기능들을 구현하는 과정이 쉽진 않았지만, 그래도 그만큼 더 기억에 많이 남습니다.
- 처음에는 흑백 필터, 밝기 보정 등의 추가 기능을 생각했으나 그림판의 기능과는 어울리지 않는다고 생각해 이미지를 업로드하여 그 위에 그림을 그릴 수 있도록 하는 기능과 더 많은 색상을 이용할 수 있는 기능, 한 번에 지울 수 있는 기능을 추가했습니다.
- 더 많은 색상을 제공하기 위한 기능을 추가하기 위해 input 태그의 type=color를 이용하여 value 값을 받아오는데 이상하게 처음에는 그 전의 색상 값을 가져오고, 한 번 더 눌러야 해당 색상 값을 가져오는 문제점이 발생했습니다. input 태그의 경우, js에서 addEventListener를 change로 해줘야 해당 이벤트가 발생하게 되는데 이 떄 이벤트리스너를 click으로 추가해서 발생한 문제였습니다. 중간 중간 console.log와 console.dir를 이용해 어디서 문제가 발생하는지를 계속 체크한 덕분에 오래 헤매지 않고 해결할 수 있었습니다.
