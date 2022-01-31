---
layout: default
title: HTML 정리하기
last_modified_date: 2020-10-24 00:10:19
parent: HTML
---

# HTML 정리하기

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

# HTML

[Hypertext-Markup-Language](https://developer.mozilla.org/ko/docs/Web/HTML)

웹을 이루는 가장 기초적인 구성 요소로 브라우저(웹 사이트)에 각 요소가 무엇을 뜻하는지 알려준다. 즉, HTML을 **시맨틱(Semantic)** 하게 작성해야 브라우저에게 알맞는 정보를 제공할 수 있다. 또한 HTML은 스크린 리더를 통해 웹페이지를 보는 것이 아니라 _듣는 사용자_ 를 위해 태그를 통해 충분한 정보를 제공해야 하며, 순서를 고려하여 작성해야 한다.

## HTML을 시맨틱(Semantic)하게 작성하려면

1. 웹 페이지의 구획 나누기
2. 각 구획마다 적절한 Sectioning Elements 정해주기
3. 최소한의 기능/의미를 갖는 가장 작은 단위로 쪼개기

### Sectioning Elements

![section](/assets/images/html/section.png)

- **section** : 논리적인 완결된 집합체에 사용, 주제별로 그룹화된 콘텐츠에 주로 사용한다.
- **nav** : 문서 간에 이동을 할 수 있는 navigation의 역할을 할 때 사용한다. ex. 메뉴
- **article** : 완결성이 있는 독립적인 내용의 경우 주로 사용한다. ex. 뉴스 기사나 블로그
- **aside** : 본문의 내용과 직접적인 연관이 없는 분리된 내용을 마크업하 때 사용한다. ex. 사이드바, 배너, 작은 위젯 등

  > ❗️ **section, nav, article, aside** 태그는 안에 **반드시** heading 태그를 사용해야 한다.

- **header** : 어떤 section의 상단부, 도입부라는 의미를 전달할 때 사용한다.
- **main** : 본문에 있어서 가장 핵심이 되는 부분을 묶어줄 때 사용한다. **_하나의 html문서에는 단 하나의 main만 사용 가능_**
- **footer** : 어떤 section 하단부라는 의미를 전달할 때 사용하고, 웹페이지의 마지막 부분에 나열하는 정보를 마크업할 때 사용한다.
  > ❗️ **header, main, footer** 태그는 heading 태그를 반드시 사용할 필요없다.

# Document

**Document Type Declaration**

= DTD 선언

= 문서 형식 선언

⇒ 이 문서가 HTML5로 작성된 문서라는 것을 브라우저에게 알려준다.

- **head** : meta 데이터를 사용하는 것들을 담는다. => 사용자가 보지 못하는 부분
- **body** : 웹 문서에서 보여주는 모든 것을 담는다. => 사용자가 볼 수 있는 부분
- **title** : 브라우저 탭에 보여지는 문서의 대제목을 보여준다 => 검색 최적화에 매우 중요하다.

  ✅ title을 잘 작성하려면 키워드를 단순 나열하기 보다는 페이지에 따라 그에 맞게 변경해야 한다.

- **link** : CSS 스타일시트를 첨부하는 태그
- **style** : HTML 문서 내에 CSS 코드를 작성할 때 쓰는 태그
- **script** : HTML 문서 내에 JavaScript 파일을 첨부할 때 사용, body의 맨 마지막 부분에 작성한다.
- **meta**
  - name : 메타데이터의 타입, 종류를 작성한다.
  - viewport : 화면 사이즈 ⇒ 디바이스 사이즈에 따라 반응할 수 있도록 한다.
  - author : 작성자를 명시할 수 있다.
  - keywords : 누군가 이 키워드를 검색하면 보여줄 수 있도록 한다.
  - description : 설명
  - content : 메타데이터의 값

> VSCODE의 emmet 기능으로 **!** 를 입력하고 엔터만 쳐도 아래 코드(CSS와 JavaScript를 적용하는 코드인 `<link rel="stylesheet" href="sytles.css">`와 `<script src="./main.js"></script>` 제외)를 작성할 수 있다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <link rel="stylesheet" href="sytles.css" />
  </head>
  <body>
    <script src="./main.js"></script>
  </body>
</html>
```

# Tag

`<name attribute="value">Content</name>`

태그는 <>로 시작해서 </>로 끝난다. 태그의 속성(attribute)을 통해 태그에 관한 추가적인 정보를 제공한다. 태그는 웹 표준에 맞게 올바르게 사용해야 한다.

[더 다양한 태그를 알고 싶다면!](https://developer.mozilla.org/ko/docs/Web/HTML/Element)

## Headings &amp; Paragraph

### Heading 제목

: 제목을 나타내는 태그로 h1, h2, h3, h4, h5, h6가 있다. h1이 가장 중요한 제목일 때 사용하고, 반대로 h6가 가장 낮은 중요도를 갖는다.

`<h1> 제목 </h1>`

### Paragraph 문단

: 하나의 문단을 나타낸다.

`<p> blabla </p>`

## Emphasis

강조할 때 사용하는 태그로 em과 strong이 있다. 보통 브라우저는 굵은 글씨로 표현한다.

### em

`<em> 강조 </em>`

### strong

`<strong> 강조 </strong>`

## Anchor

현 위치에서 다른 위치로 이동할 때 사용하는 링크 태그

`<a href="주소" 링크 </a>`

> **href 주소값 표기 방법**
>
> 1.  웹 URL
> 2.  페이지 내 이동 => 이동하고 싶은 태그의 id를 이용한다.
>
> ```html
> <a href="#hello"> Go to hello </a>
> <section id="hello"></section>
> ```
>
> 3.  메일 쓰기
>     `<a href="mailto:메일주소"> </a>`
> 4.  전화 걸기
>     `<a href="tel:전화번호"> </a>`
>
> 💡 페이지를 이동할 때 원래 페이지가 사라지고 이동하는 것이 아닌 **새 창으로** 열고 싶을 때
> `<a href="주소" target="_blank"> </a>`

## Image

`<a href="주소" target="_blank"> </a>`

- **src** = source : 이미지 파일의 경로 또는 웹사이트에서 이미지의 주소값을 전달한다.

  `<img src="https://bit.ly/2mvVsGI" alt="두 마리의 강아지"/>`

- **alt** = alternative text : 대체 텍스트 ⇒ 인터넷이 느려서 이미지가 보이지 않을 때나 시각장애인을 위해 이미지 자리에 텍스트를 쓴다.

  `<img src="./img/puppy.jpg" alt="강아지"/>`

  ❗️img가 의미가 없거나 아무 정보가 없다면 alt값을 삭제하는 것이 아니라 비워둔다. (alt="")

  그리고 img가 진짜 중요하지 않다면 굳이 html에서 작성할 필요 없다. (CSS로 작성 가능하기 때문에)

## Lists

목록을 표현할 때 사용한다. **ul과 ol의 자식요소는 _무조건 li_ 만 가능하다.**

비슷한 구조의 항목들이 (병렬적으로) 나란히 연결되어 있을 때 주로 사용한다. (ex. dropdown menu)

- **li** = list item : 리스트의 항목을 표시할 때 사용한다.
- **ol** = ordered list : 순서가 중요한 목록
- **ul** = unordered list : 순서가 중요하지 않은 목록

> 만약 list가 링크로 연결한다면 ul, ol 태그 안에 li 태그가 있고 그 안에 a 링크 태그를 넣어야 한다. a링크 태그 안에 li 태그를 넣으면 ul과 ol의 자식요소가 li가 아닌게 생기므로 문법적으로 맞지 않다.

## Description List

정의 리스트인 dl은 용어를 정의할 때, key-value로 정보를 제공할 때 사용한다.

dl의 자식요소는 dt, dd, div만 가능하다.

키워드 형태로 정보가 제공될 때 주로 사용한다. (ex. 100 posts => 100 = dd, posts = dt)

- **dl** = description list
- **dt** = description term : 이름에 해당하는 것을, key에 해당하는 것인 용어를 마크업할 때 쓰는 태그
- **dd** = description data : 이름, key에 해당하는 정보를 마크업할 때 쓰는 태그
- **dfn** = definition : dt태그 안에 좀 더 사전적으로, 의미를 구체적으로 전달하고 싶을 때 사용한다.

```html
<dl>
  <div>
    <dt>HTML</dt>
    <dd>HTML에 대한 설명</dd>
  </div>
  <div>
    <dt>CSS</dt>
    <dd>CSS에 대한 설명</dd>
  </div>
  <dl></dl>
</dl>
```

> ❗️ dt와 dd는 함께 사용해야 한다. 또한 반드시 dl의 자식요로만 존재해야 한다. (단독으로 사용 불가능)

## Quotations

인용할 때 사용하는 태그에는 blockquote와 q가 있다.

- **blockquote** : 문단이나 내용 전체가 하나의 인용문일 경우 사용한다. (q보다 더 많이 사용)

  (+) 출처는 blockquote 태그의 cite 속성으로 제공한다. 이 경우 출처는 사용자에게 보이지 않는다.

  출처 텍스트는 blockquote 태그 안에 요소로 제공할 수 있다. 이 경우 사용자에게 출처가 보인다.

- **q** = quotation : p 태그 안에서 인용한 문장을 q태그로 감싸주면 나중에 "" 따옴표 처리가 되어서 나타난다.

## Div & Span

Div와 Span은 레이아웃에 아무런 영향을 주지 않는 컨테이너의 역할을 하는 태그로 Non Semantic 태그이다.주로 스타일을 적용하기 위해 사용한다.

💡 div 태그와 span 태그는 둘 다 거의 똑같지만, 텍스트를 넣을 때는 주로 span 태그를 사용한다.

## Form

사용자로부터 인풋(Input)을 받기 위한 태그

`<form action="" method=""> </form>`

- **action** = "API 주소" : form을 처리해 줄 API 서버쪽 친구에게 접근 가능한 URL로 마크업할 때는 일단 #으로 작성한다.
- **method** = "GET or POST"
  - GET : POST를 쓰지 않는 경우에 사용한다.
  - POST : 중요한 정보나 정보의 양이 많을 때 사용한다.

### Input

기본적인 입력창

**Input type**

- email : input에 이메일을 적지 않으면 이메일을 작성하라는 경고가 발생한다. (값에 @가 없으면 에러)

- password : 비밀번호는 입력해도 ・처리 되어서 실제 값은 보이지 않는다.

- url
- number : 숫자만 입력 가능하다.

  - min : 숫자의 최소값을 입력 (글자 수와는 상관 없음)
  - max : 숫자의 최대값을 입력

  ```html
  <form action="" method="GET">
    <input type="number" min="1" max="100" placeholder="나이를 입력하세요" />
  </form>
  ```

- file : 파일을 선택하거나 업로드할 수 있도록 한다.

  - accept : 허용하고자 하는 파일의 확장자를 결정한다.
    - image/\* ⇒ 모든 이미지를 허용한다는 뜻
    - video/\* ⇒ 모든 비디오를 허용한다는 뜻
  - multiple : 여러 개의 파일을 업로드할 수 있도록 만드는 속성

  ```html
  <form action="" method="GET">
    <input type="file" accept=".png, .jpg" />
  </form>
  ```

- radio &amp; checkbox : radio는 오로지 하나의 항목만 선택 가능하지만, checkbox 다중 선택이 가능하다.

  - Radio &amp; Checkbox Attribute
    - id : radio/checkbox의 이름을 label 태그로 지정할 수 있는데, label태그와 radio/checkbox태그를 서로 연관지어 주기 위해서 같은 id를 사용해야 한다
    - name : 같은 그룹의 radio/checkbox태그 라는 것을 알려주기 위해 사용한다. radio 태그에서 name 속성을 사용하지 않는 경우, 같은 radio 그룹이라는 것을 인식하지 못해서 하나가 아닌 여러 개를 선택할 수 있는 에러가 발생하기 때문.
    - value : 선택 항목이 가지는 고유한 값으로 하나를 선택해서 '제출(submit)'버튼을 누르면 value에 지정한 값이 서버로 전송된다. 즉, 어떤 값이 선택되었는지 구분하기 위해서 사용해야 한다

  ```html
  <form action="" method="GET">
    <input type="radio" name="icecream" id="chocolate" value="chocolate" />
    <label for="chocolate">초콜릿 맛</label>
    <input type="radio" name="icecream" id="vanila" value="vanila" />
    <label for="vanila">바닐라 맛</label>
    <button type="submit">Submit</button>
  </form>
  ```

  ```html
  <form action="" method="GET">
    <input type="checkbox" name="skills" id="html" value="html" />
    <label for="html">HTML</label>
    <input type="checkbox" name="skills" id="css" value="css" />
    <label for="css">CSS</label>
    <input type="checkbox" name="skills" id="js" value="js" />
    <label for="js">JavaScript</label>
    <button type="submit">Submit</button>
  </form>
  ```

**Input Attribute**

```html
<form action="" method="GET">
  <input
    type="text"
    placeholder="아이디를 입력하세요"
    maxlength="10"
    minlength="5"
    required
    value="dwon424"
  />
</form>
```

- placeholder : 아무것도 값이 없을 때 기본적으로 보여주는 text
- maxlength : 받는 정보의 제한하고 싶은 숫자를 작성한다. ex. 아이디는 14자리 이하
- minlength : 받는 정보의 최소 숫자를 작성한다. ex. 아이디는 8자리 이상
- required : 무조건 입력을 해야 하는 input으로 만들어준다.
- disabled : 사용자가 입력을 할 수 없게 input을 막는다.
- value : 일종의 초기값

> 💡 value는 기본적으로 입력되어 있는 값으로 복사 가능하지만, placeholder는 input에 관한 정보이므로 복사할 수 없다.

### Label

부가적인 태그로 input에 관한 제목이다. 라벨을 클릭하면 이름에 focus가 생긴다.

해당 라벨이 해당 input의 제목이라는 것을 알려주기 위해 label for 값과 input id값은 같은 값을 사용한다.

```html
<label for="user-name">이름</label> <input type="text" id="user-name" />
```

### Select &amp; Option

- Select : option 메뉴를 제공하는 태그
- Option : select 태그의 pulldown menu를 만들기 위해 사용한다.
- Select &amp; Option Attribute
  - name : 같은 그룹의 select라는 것을 알려주기 위해 사용한다.
  - value : 선택하고 제출했을 때 서버에 어떤 값이 선택되었는지 알려주기 위해 사용한다.
  - multiple : option으로 만든 pulldown menu 중 다중 선택 가능하다.

```html
<form action="" method="GET">
  <label for="skill">Skill</label>
  <select multiple name="skills" id="skill">
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="js">JavaScript</option>
  </select>
  <button type="submit">Submit</button>
</form>
```

### Textarea

여러 줄에 거쳐서 많은 양의 텍스트를 전달 받을 때 사용한다.

- Textarea Attribute
  - rows : 가로로 몇 칸 정도 쓸 수 있는지
  - cols : 세로로 몇 줄 정도 쓸 수 있는지

> `input type="text"/` ⇒ 보통 한 줄 정도의 텍스트를 받을 때 사용하고, textarea는 여러 줄의 텍스트를 받을 때 사용한다.

### Buttons

- Button type
  - button : 눌렀을 때 여러 기능을 넣을 수 있어 만만하게 쓸 수 있다.
  - submit : 버튼의 용도가 form을 제출할 때 사용한다면 그 때 사용한다.
  - reset : 작성한 form을 다시 쓰고싶을 때만 사용한다. (잘 사용하지 않는다.)

> Button과 `input type="submit"`은 기능적 차이는 없으나, button이 더 풍부한 렌더링 옵션을 제공한다.

## Table

데이터를 담은 표를 만들 때 사용하는 태그로 주로 같은 구조가 병렬적으로 반복될 때 사용한다.

- **tr** = table row : 표의 가로줄을 만드는 역할로 tr 안에 있는 th, td는 가로로 배치된다.

- **th** = table header : 표의 제목을 나타내며, 처음에 적는 th에 따라 나중에 적는 td가 영향을 받게 된다. (th와 td의 항목 수가 일치해야 하기 때문)

- **td** = table data : 셀을 만드는 역할로 표의 셀 안에 들어가는 데이터를 적으며, 데이터가 없는 경우에는 td의 값을 빈칸으로 비워둔다.

- **thead** : 꼭 사용할 필요는 없지만 th를 thead로 감싸주면 브라우저가 table head라 더 명확하게 인식한다.

- **tbody** : 꼭 사용할 필요는 없지만 td를 tbody로 감싸주면 브라우저가 table body라 더 명확하게 인식한다.

- **tfoot** :꼭 사용할 필요는 없지만, 마지막에 요약하는 결론의 부분이 필요할 때 사용한다. ex. 총 합계

> 보통 table태그의 thead는 한 번 만들고, tbody는 여러 개 만들 수 있어 잘 만들어두면 계속 복사해서 사용할 수 있다.

- **td rowspan="숫자"** : 한 칸이 아닌 _세로_ 의 여러 칸을 차지할 때 쓰는 속성

  - 만약 A라는 값이 2칸을 차지했다면 처음 td에 A를 쓸 때 rowspan 속성을 사용하고(td rowspan="2"), 그 다음줄의 같은 순서의 td는 생략한다. 이미 rowspan을 사용했을 때 그 다음 A를 같이 쓴다는 의미를 포함하는 것이기 때문.

- **td colspan="숫자"** : 한 칸이 아닌 _가로_ 의 여러 칸을 차지할 때 쓰는 속성

- **th scope="row or col"** : table header에만 적을 수 있는 태그로 브라우저가 th의 성격에 대해 명확하게 알 수 있도록 한다.

  - col : 세로줄에 대한 header일 때
  - row : 가로줄에 대한 header일 때

![table](/assets/images/html/table.png)

```html
<table>
  <thead>
    <tr>
      <th></th>
      <th scope="col">Sat</th>
      <th scope="col">Sun</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1st period</th>
      <td rowspan="2">HTML</td>
      <td>JavaScript</td>
    </tr>

    <tr>
      <th scope="row">2nd period</th>
      <!-- <td>HTML</td> -->
      <td rowspan="2">CSS</td>
    </tr>
    <tr>
      <th scope="row">3rd period</th>
      <td>JavaScript</td>
      <!-- <td>CSS</td> -->
    </tr>
    <tr>
      <th colspan="3" scope="row">Lunch</th>
    </tr>
  </tbody>
</table>
```

## Media

### Audio

오디오 파일을 첨부할 때 사용하는 태그

```html
<audio src=""></audio>
```

```html
<audio controls>
  <source src="" type="" />
</audio>
```

- Audio Attributes

  - controls : 사용자가 오디오를 조절할 수 있도록 한다.
  - autoplay : 브라우저를 켜면 자동으로 플레이 되는 속성. 하지만 현재는 크롬 브라우저에서 소리 있는 영상의 자동 재생이 안되기 때문에 소리가 있는 영상을 autoplay하려면 muted 속성도 함께 적용해야 한다.
  - loop autoplay : 자동 재생의 무한 반복
  - source : audio에서 src 속성 대신 source 태그를 따로 사용하는 이유는 같은 음원의 다른 확장자를 넣을 수 있기 때문이다. 예를 들어, 첫 번째 음악인 wav 확장자가 사용자의 브라우저에서 지원하지 않는다면, mp3를, 마지막으로 ogg를 재생할 수 있도록 해준다.

    - source type은 [html audio](https://developer.mozilla.org/ko/docs/Web/HTML/Element/audio) 여기서 확인해서 쓰기

    ```html
    <audio controls>
      <source src="./audio/kimbug.wav" type="audio/wav" />
      <source src="./audio/kimbug.mp3" type="audio/mpeg" />
      <source src="./audio/kimbug.ogg" type="audio/ogg" />
      <p>브라우저를 업데이트하시는게 어떠실까요?</p>
      <a href="https://browsehappy.com">브라우저 업데이트하기</a>
    </audio>
    ```

### Video

비디오 파일을 첨부할 때 사용하는 태그

❗️ video 태그에 css를 적용하기 위해 class를 적을 때는 video 자체에 적는 것이 아니라 div로 감싸주고 감싼 div에 class를 적용해야 한다.

```html
<video src=""></video>
```

```html
<video controls>
  <source src="" type="" />
</video>
```

- Video Attributes

  - controls : 사용자가 비디오를 조절할 수 있도록 한다.
  - autoplay : 브라우저를 켜면 자동으로 플레이 되는 속성이지만, 현재 크롬 브라우저에서 소리 있는 영상의 자동 재생이 안되기 때문에 소리가 있는 영상을 autoplay하려면 muted 속성도 함께 적용해야 한다.
  - loop autoplay : 자동 재생의 무한 반복
  - source : video에서 src 속성 대신 source 태그를 따로 사용하는 이유는 같은 영상의 다른 확장자를 넣을 수 있기 때문이다. 예를 들어, 첫 번째 비디오인 mov 확장자가 사용자의 브라우저에서 지원하지 않는다면, mp4를 재생할 수 있도록 한다.

    - type은 [html video](https://developer.mozilla.org/ko/docs/Web/HTML/Element/source) 여기서 확인해서 쓰기

    ```html
    <video controls>
      <source src="./audio/kimbug.mov" type="video/quicktime" />
      <source src="./audio/kimbug.mp4" type="video/mp4" />
      <p>브라우저를 업데이트하시는게 어떠실까요?</p>
      <a href="https://browsehappy.com">브라우저 업데이트하기</a>
    </video>
    ```

### iframe

하나의 또 다른 HTML 문서를 현 페이지에 embed하는 것으로 보통 youtube, vimeo 등 외부 콘텐츠를 embed하여 공유할 때 자주 사용한다. ifrmae의 값은 보통 영상에서 제공하기 때문에 직접 작성할 일은 거의 없다.

```html
<iframe src="embed하고 싶은 페이지의 주소"></iframe>
```

## Address

(물리적) 주소, url, email, 전화번호, SNS 등이 사용 가능하다.

```html
<address>연락처</address>
```

```html
<address>
  <h1>네이버</h1>
  <a href="https://www.naver.com"></a>
</address>
```

## Abbreviation

약자, 약어를 사용자가 모를 경우에 따로 감싸주어서 알려주는 태그 (p태그 등 다른 태그의 내용 안에서도 사용 가능하다.)

```html
<abbr title="Attention Deficit Hyperactivity Disorder">ADHD</abbr>
```

## Code &amp; Prefomatted text

html 문서 상에서 코드를 작성하고 싶을 때 사용한다.

- **pre** : 이미 포맷이 정해진 텍스트 ⇒ 작성한 그대로가 브라우저에 표현된다.
  ```html
  <pre>
   ㅇ ㅏ ㄴ ㅕ ㅎ ㅏ ㅅ ㅔ ㅇ
    ㄴ    ㅇ            ㅛ
  </pre>
  ```
- **code** : 코드를 작성하고 싶을 때 사용하는데, 보통 pre 태그 안에 code 태그를 작성해서 사용한다. (특히 여러 줄의 태그를 작성하고 싶을 때 pre코드를 같이 사용하고, 한 줄의 짧은 코드는 굳이 pre태그를 같이 사용할 필요없다.)

## br

줄바꿈 태그로 이러한 태그를 닫힌 태그라 한다. (`<br>`이나 `` 둘 다 사용 가능하지만 하나만 사용해야 된다.)

```html

```

## hr

구분선 태그 : 문단과 문단을 나눌 때 수평선을 그려준다.

```html
<hr />
```

## blockquote

인용구문을 넣을 때 쓰는 태그

```html
<blockquote>인용구문</blockquote>
```

# More..

## WAI-ARIA

인터넷의 접근성을 높일 수 있는 API로 시각장애인 등 웹 페이지를 보는 것이 아닌 _듣는 사용자가 스크린리더를 이용할 경우_ 이미지의 명확한 이유를 알려주기 위해서 사용한다.

- aria-label : 우리 눈에 보이지 않더라도 브라우저에게는 전달이 되면 좋은 정보, 혹은 스크린 리더를 통해 웹을 사용하는 사용자들에게 전달해야 하는 정보를 제공하고 싶을 때 사용한다. 즉, 스크린 리더에게 좀 더 구체적으로 '이 요소는 이렇게 읽어 달라'라고 구체적으로 명령할 때 사용한다.

  ```html
  <a href="#" **aria-label**="Go to previous page">Previous</a>
  ```

  > 또는 전달되어야 하는 정보의 class를 sr-only를 지정해 css에서 sr-only를 화면에서 보이지 않게 처리하면 화면에서는 보이지 않지만, 브라우저에는 정보가 전달되기 때문에 스크린 리더를 통해 읽을 경우에는 정보가 전달된다.
  >
  > => 💡 **_aria-label과 sr-only는 기능의 차이_** 는 거의 없고, 스타일의 차이만 있다.

- aria-hidden="true" : 사용자에게 전달되지 않아도 되는 정보(스크린 리더를 통해 읽을 필요 없는 정보)일 때 사용한다.

  ```html
  <span aria-hidden="true"></span>
  ```
