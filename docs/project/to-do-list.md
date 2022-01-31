---
layout: default
title: TO DO LIST
last_modified_date: 2021-02-15 17:02:39
parent: Projects
---

# TO DO LIST

![todolist](/assets/images/project/todolist.gif)

## Introduction

Vanlia JavaScript로 만들어본 [blink](https://apps.apple.com/kr/app/blink-%EB%B9%A0%EB%A5%B8%EB%A9%94%EB%AA%A8-%EB%B8%94%EB%A7%81%ED%81%AC/id1182856129)를 닮은 TO DO LIST 입니다.

깔끔한 것을 좋아해서 단순하면서도 기본 기능에 충실한 TO DO LIST를 만들어보고 싶었습니다. 평소에 자주 사용하는 어플리케이션인 blink는 웹에서는 따로 사용할 수가 없기 때문에 개인적으로 웹에서 사용할 수 있는 TO DO LIST도 좋겠다는 생각에 현재 프로젝트를 진행했습니다.

## Peroid

2021.02.14

## Stack

- HTML
- CSS
- JavaScript

## TO DO LIST 기능

- 오늘의 날짜를 확인할 수 있습니다.
- 할 일을 추가하면 진한 회색 영역에 추가됩니다.
- 할 일을 클릭하면 완료되었다는 뜻으로 연한 회색 영역으로 이동합니다.
- 연한 회색 영역에 있는 할 일을 다시 클릭하면 진한 회색 영역에 추가할 수 있습니다.
- 할 일을 삭제하고 싶다면 할 일 위에 마우스를 올렸을 때 보이는 삭제 버튼을 누르면 삭제됩니다.
- LocalStorage에 저장되기 때문에 새로고침을 해도 기존에 추가된 to do list가 삭제되지 않습니다.

## 아쉬운 점

- 추가된 할 일을 수정할 수 없습니다. 필수 기능이 아니라는 생각에 시간 부족으로 제외했던 기능이지만, 다음에 업데이트할 때 수정 기능을 추가할 계획입니다.
- blink 어플처럼 할 일을 클릭할 때마다 색이 변하는 기능을 추가하고 싶었습니다. 하지만 현재는 클릭했을 때 할 일을 완료되는 기능이므로 어떻게 추가해야될지 애매해서 추가하지 못했습니다.
- 작은 프로젝트라는 생각에 충분한 기획을 하고 시작하지 않아서 중간 중간 수정 작업이 많았습니다. 예를 들어서, 처음에는 삭제 버튼이 무조건 할 일 옆에 같이 보이는 디자인이었지만, 마음에 들지 않아서 결국 hover했을 때만 보이도록 다시 수정했습니다.

## Link

- [To Do List](https://2dowon.github.io/Project-ToDoList/)
- [GitHub](https://github.com/2dowon/Project-ToDoList)
