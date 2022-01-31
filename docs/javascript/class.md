---
layout: default
title: Class
last_modified_date: 2021-02-09 16:02:24
parent: JavaScript
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Class</div>

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

class는 관련이 있는 변수나 함수들을 묶어 놓는 것을 말한다. 즉, 클래스는 fields와 methods로 구성되는데, fields 데이터로만 구성된 클래스도 많이 있다. 이와 같은 클래스는 데이터 클래스라고도 한다.

### Class = fields + methods

> person이라는 class 안에는 name과 age라는 속성(field)과 speak라는 행동(method)

```jsx
class person {
  name;
  age;
  speak();
}
```

### Class와 Object의 차이점

**Class**

- template
- declare once 한번만 선언 가능
- no data in 클래스 자체에는 데이터가 없다
- ex. 붕어빵 틀

**Object**

- instance of a class
- created many times 많이 만들 수 있다
- data in ⇒ 붕어빵 재료(팥, 슈크림 등)
- ex. 팥 붕어빵, 슈크림 붕어빵
- 클래스를 이용해서 데이터를 넣어 만드는 것

## Class 선언

> Person 클래스 선언

```jsx
class Person {
  // constructor
  constructor(name, age) {
    // fields
    this.name = name;
    this.age = age;
  }
  // methods
  speak() {
    console.log(`${this.name}: hello!`);
  }
}
```

## Object 생성

> Person 클래스를 이용해서 dowon이라는 object 생성

```jsx
const dowon = new Person("dowon", "26");
console.log(dowon.name); // dowon
console.log(dowon.age); // 26
dowon.speak(); // dowon: hello!
```

## Getter & Setter

### Getter & Setter를 쓰는 이유

- Getter & Setter는 사용자가 잘못 사용하더라도 방어적인 자세로 만들기 위해서 사용한다.
- get을 통해 값을 리턴하고, set을 통해 값을 설정한다.
- 예를 들어서, 커피 자판기가 있다. 커피 자판기에는 커피의 수가 있고, 자판기에 동전을 넣으면 커피를 만들어준다. 이 때 커피의 종류가 -1이 될 수 있을까? 안된다. 커피의 수는 아무리 적어도 0이여야 하기 때문에 사용자가 실수로 커피의 종류를 -1로 입력하더라도 우리는 이를 다시 0으로 설정해줄 필요가 있다. 물론 자판기의 경우에는 처음부터 사용자가 커피의 수를 건들일 수 없도록 private로 설정하는 encapsulate 캡슐화가 필요하다.
- 이 예시에서 coffee vending machine(커피 자판기)는 class이고, a number of coffee, put coins, make coffees는 자판기의 property이다. 사용자가 입력하는 a number of coffee를 get으로 얻어서 만약 사용자가 잘못 입력했다면 이를 set으로 수정한다.

> 사용자가 나이를 마이너스 값으로 잘못 입력하더라도 set을 이용해 0으로 수정한다

```jsx
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  get age() {
    return this._age;
  }

  set age(value) {
    // if (value < 0) {
    //   throw Error("age can not be negative");
    // }
    this._age = value < 0 ? 0 : value;
    // this._age = valu;
  }
}

const user1 = new User("Steve", "Job", -1);
console.log(user1.age); // 0
```

- `_age` 처럼 변수명을 바꾸는 이유
  ⇒ getter와 setter에서 age라는 같은 변수명을 사용하면 계속 반복되면서 콜스택이 다 차버리는 에러가 생기기 때문에 변수명을 바꿔줘야 한다. 보통은 `_`를 붙여서 사용한다. 따라서 위 예시처럼 field에는 \_age이지만, age라고 호출 가능하다.
- `if (value < 0) {throw Error("age can not be negative");}` 이렇게 직접적으로 나이가 마이너스일 때 에러를 표시해도 좋지만, 이는 너무 공격적인 방법이므로 `this._age = value < 0 ? 0 : value;` 처럼 마이너스면 자동으로 0으로 만들어주도록 하는 방법을 사용하는 것이 좋다.

## Fields (public, private)

constructor 생성자를 사용하지 않고, field를 정의할 수 있는 방법이다.

- `publicField` : 외부에서 접근이 가능하다
- `#privateField` : class내부에서만 값이 보여지고, 접근이 가능. 외부에서는 읽을 수도, 변경할 수도 없다

```jsx
class Experiment {
  publicField = 2;
  #privateField = 0;
}
const experiment = new Experiment();
console.log(experiment.publicField); // 2
console.log(experiment.privateField); // undefined
```

> 2021.02 기준으로 IE, Safari를 제외하고 거의 모든 사이트에서 지원되고 있다.

![class1](/assets/images/javascript/class1.png)

[Public class fields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields)

## Static

object의 데이터와 상관없이 공통적으로 class에서 사용하는 것이라면 static을 이용해서 class자체에 연결할 수 있다. Static을 이용하면 메모리의 사용을 조금 더 줄여줄 수 있다.

⚠️ static 함수를 호출할 때는 object의 이름이 아닌 class의 이름을 이용해서 호출해야 된다.

```jsx
class Article {
  static publisher = "Dream Coding";
  constructor(articleNumber) {
    this.articleNumber = articleNumber;
  }
  static printPublisher() {
    console.log(Article.publisher);
  }
}
const article1 = new Article(1);
const article2 = new Article(2);
console.log(article1.publisher); // undefined
console.log(Article.publisher); // Dream Coding
Article.printPublisher(); // Dream Coding
```

> 2021.02 기준으로 IE, Safari를 제외하고 거의 모든 사이트에서 지원되고 있다.

![class2](/assets/images/javascript/class2.png)

[static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)

## 상속(Inheritance)과 다형성(Polymorphism)

- **상속과 다형성을 쓰는 이유**
  ⇒ 예를 들어, 다양한 도형을 만들 때, 하나씩 만들기 보다는 공통적인 속성을 shape으로 묶어서 만들면 코드도 간결해지고, 그렇기에 나중에 유지보수하기도 좋기 때문이다.
- `extends`(연장한다)를 이용해서 class를 연장해 속성을 가져올 수 있음
- `overwriting`(필요한 함수만 재정의해서 사용 가능) 으로 재정의하게 되면 부모의 함수는 더 이상 호출되지 않게 된다. 따라서 부모의 함수도 같이 호출하고 싶으면 `super`를 이용해야 된다.

> Shape 클래스를 상속받아 Rectangle, Triangle 클래스 선언

```jsx
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }
  draw() {
    console.log(`drawing ${this.color} color of`);
  }
  getArea() {
    return this.width * this.height;
  }
}

class Rectangle **extends** Shape {}
class Triangle **extends** Shape {
  draw() {
    **super**.draw();
    console.log("🔺");
  }
	// 삼각형은 사각형과 넓이를 구하는 공식이 다르므로 재정의해야 한다
  getArea() {
    return (this.width * this.height) / 2;
  }
}

const rectangle = new Rectangle(20, 20, "blue");
rectangle.draw();
console.log(rectangle.getArea()); // drawing blue color of / 400

const triangle = new Triangle(20, 20, "red");
triangle.draw();
console.log(triangle.getArea()); // drawing red color of / 🔺 / 200
```

## (+) instanceOf

- instanceOf를 이용하면 왼쪽에 있는 object가 오른쪽에 있는 class의 instance인지 아닌지를 확인할 수 있다. 다시 말해, 이 object가 이 class를 이용해서 만들어진 아이인지 아닌지를 확인하는 것이다.
- true와 false로 값을 리턴한다
- ✅ JavaScript의 모든 object들은 Object로부터 상속한 것이다

```jsx
console.log(rectangle instanceof Rectangle); // true
console.log(triangle instanceof Rectangle); // false
console.log(triangle instanceof Triangle); // true
console.log(triangle instanceof Shape); // true
console.log(triangle instanceof Object); // true
```

# Ref.

- [자바스크립트 6. 클래스와 오브젝트의 차이점(class vs object), 객체지향 언어 클래스 정리 - 프론트엔드 개발자 입문편 (JavaScript ES6)](https://www.youtube.com/watch?v=_DLhUBWsRtw&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=6)

- [Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)
