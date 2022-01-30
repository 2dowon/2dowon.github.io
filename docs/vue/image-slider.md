---
layout: default
title: Vue에서 image slider 구현하기
parent: Vue
nav_order: 1
---

![ImageSlider](/assets/images/vue/imageSlider.gif)

이미지 슬라이더를 만드는 일은 생각보다 자주 있는데, 막상 구현하려고 보면 막막해서 보통 라이브러리를 이용해서 구현하는 경우가 많다. 하지만 바닐라 JS를 이용해서도 생각보다 쉽게 구현할 수 있다!!

- 라이브러리 없이 바닐라 JS를 이용해서 구현했지만, 현재 프로젝트에서 vue를 사용하고 있기 때문에 개인적으로는 vue 예제를 가지고 왔습니다.
  만약 바닐라 JS로만 된 예시를 원하신다면 [여기](https://penguingoon.tistory.com/257)를 클릭해주세요! 저도 이 글을 참고해서 작성했습니다!
- 현재 이미지 슬라이더는 **touch event를 이용해 구현했기에 모바일 환경에서만 작동**합니다.
- 기본적으로는 이미지가 모바일 화면에 가득찰 수 있도록 2000px의 이미지를 사용했고, 최대 크기도 2000px로 설정했습니다.
- 만약 모바일 화면보다 작은 사이즈의 이미지 크기를 원한다면, style(css)의 image-album 클래스와 image 클래스의 max-width, max-height 값을 원하는 값으로 설정해주면 됩니다.

### App.vue

```jsx
<template>
  <div class="image-album">
    <div class="images">
      <img
        class="image"
        v-for="imageUrl in imageUrls"
        v-bind:key="imageUrl.index"
        v-bind:src="imageUrl"
      />
    </div>
    <div v-if="imageUrls.length > 1" class="image-circle-wrapper">
      <div
        class="image-circle"
        v-for="(imageUrl, index) in imageUrls"
        v-bind:key="imageUrl.index"
        v-bind:class="{ activeImg: index === curPos }"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "imageSlider",
  data() {
    return {
      curPos: 0,
      postion: 0,
      start_x: 0,
      end_x: 0,
      IMAGE_WIDTH: 0,
      images: null,
      imageUrls: [
        "https://www.shutterstock.com/blog/wp-content/uploads/sites/5/2019/09/shutterstock_1151676383.jpg?w=2000",
        "https://www.shutterstock.com/blog/wp-content/uploads/sites/5/2019/09/shutterstock_1151632343.jpg?w=2000",
        "https://www.shutterstock.com/blog/wp-content/uploads/sites/5/2019/09/shutterstock_1429964489.jpg?w=2000",
      ],
    };
  },
  computed: {
    getImageWidth: () => {
      const imgWidth = document.querySelector(".images").offsetWidth;
      return imgWidth;
    },
  },
  mounted: function () {
    this.IMAGE_WIDTH = this.getImageWidth;
    this.images = document.querySelector(".images");
    this.images.addEventListener("touchstart", this.touch_start);
    this.images.addEventListener("touchend", this.touch_end);
  },
  methods: {
    prev() {
      if (this.curPos > 0) {
        this.postion += this.IMAGE_WIDTH;
        this.images.style.transform = `translateX(${this.postion}px)`;
        this.curPos = this.curPos - 1;
      }
    },
    next() {
      if (this.curPos < this.imageUrls.length - 1) {
        this.postion -= this.IMAGE_WIDTH;
        this.images.style.transform = `translateX(${this.postion}px)`;
        this.curPos = this.curPos + 1;
      }
    },

    touch_start(event) {
      this.start_x = event.touches[0].pageX;
    },

    touch_end(event) {
      this.end_x = event.changedTouches[0].pageX;
      if (this.start_x > this.end_x) this.next();
      else this.prev();
    },
  },
};
</script>

<style scopped>
.image-album {
  width: 100%;
  height: auto;
  max-width: 2000px;
  max-height: 2000px;
  overflow: hidden;
}
.images {
  position: relative;
  display: flex;
  height: auto;
  transition: transform 0.5s;
}
.image {
  width: 100%;
  height: auto;
  max-width: 2000px;
  max-height: 2000px;
}
.image-circle-wrapper {
  display: flex;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -18px);
}
.image-circle {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: white;
  border: 1px solid #d2d2d2;
  margin-right: 12px;
}
.image-circle:last-child {
  margin-right: 0;
}
.image-circle.activeImg {
  background-color: #404040;
}
</style>
```

# Ref.

- [Dogs in Design: 10 Canine-Themed Projects That You'll Love](https://www.shutterstock.com/blog/dog-themed-design-projects)

- [자바스크립트 모바일 터치 슬라이더 구현하기 (feat. 터치 이벤트)](https://penguingoon.tistory.com/257)
