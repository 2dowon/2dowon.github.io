---
layout: default
title: iPhone에서 video가 전체화면 모드로 넘어가는 이슈
last_modified_date: 2022-01-27 17:01:13
parent: HTML
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">iPhone에서 video가 전체화면 모드로 넘어가는 이슈</div>

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

먼저 video를 재생하기 위해서는 video 파일을 첨부할 수 있는 `video` 태그를 이용하는 방법이 있다.

일반적으로 video 태그를 이용해 video를 재생할 경우 PC나 안드로이드에서는 제대로 재생이 되지만, 아이폰에서 제대로 재생되지 않고 전체화면 모드로 넘어가서 재생되는 문제가 있었다. 이를 해결하는 방법은 아래와 같다.

## iPhone에서 video가 전체화면 모드에서 재생되는 문제점

⇒ video 태그에 `playsinline` 속성을 추가해주면 해결할 수 있다.

iOS 비디오 정책에 따라 `playsinline` 속성 없이 video를 iPhone에서 재생할 경우 전체 화면 모드가 필요하다. 따라서 전체화면 모드에서 영상을 재생하고 싶지 않은 경우 `playsinline` 속성을 video 태그에 인라인으로 추가함으로써 해결할 수 있다.

## + video 태그 속성 정리

```html
<video controls loop autoplay muted playsinline src="">
  <source src="" type="" />
  <!-- 필요하다면 -->
</video>
```

- `controls` : 사용자가 비디오를 조절할 수 있도록 ⇒ 아래 이미지처럼 비디오 재생, 음량 등과 관련한 제어창을 보여준다.

  ![video](/assets/images/html/video.png)

- `muted` : 음소거
- `autoplay` : 브라우저를 켜면 자동으로 플레이 되는 속성
  - 하지만 현재는 크롬에서 소리 있는 영상의 자동 재생이 안된다. 따라서 소리가 있는 영상을 autoplay하려면 muted 속성도 함께 적용해야 한다.
- `loop autoplay` : 자동 재생의 무한 반복
- `playsinline` : iPhone에서는 비디오를 재생할 경우 전체 화면으로 재생된다. 만약 전체 화면으로 재생되기를 원하지 않는다면 이 속성을 추가해서 막을 수 있다.
- `source` : video에서 src 속성 대신 source 태그를 따로 사용하는 이유는 같은 영상의 다른 확장자를 넣을 수 있기 때문이다.

  > 첫 번째 비디오인 mov 확장자를 유저의 브라우저에서 지원하지 않는다면, mp4를 재생할 수 있도록

  ```html
  <video controls>
    <source src="./test.mov" type="video/quicktime" />
    <source src="./test.mp4" type="video/mp4" />
  </video>
  ```

# Ref.

- [비디오 삽입 요소 - HTML: Hypertext Markup Language ](https://developer.mozilla.org/ko/docs/Web/HTML/Element/Video)

- [[HTML5] 20강 playsinline - iOS비디오정책](https://ossam5.tistory.com/155)
