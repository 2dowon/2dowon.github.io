---
layout: default
title: Redux 사용하기
last_modified_date: 2021-11-13 20:11:06
parent: React
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Redux 사용하기</div>

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

React를 이용한 프로젝트의 규모가 커지면서 상태관리를 위한 라이브러리를 알아보기 시작했다. React 전역 상태관리를 위한 라이브러리에는 대표적으로 Redux, Mobx, Recoil이 있다. 최근에는 페이스북에서 만든 Recoil이 뜨고 있다고는 하지만, 아직 나는 상태관리를 위한 라이브러리를 하나도 사용해보지 않은 만큼 사람들이 가장 많이 쓰고 있고 그래서 정보가 가장 많은 Redux부터 사용해보기로 했다.

> [npm trends](https://www.npmtrends.com/mobx-vs-recoil-vs-redux)를 통해 상태관리 라이브러리를 확인해보면 압도적으로 Redux를 많이 사용하고 있음을 알 수 있다.

![redux](/assets/images/react/redux.png)

# Redux

> Redux 작동 원리

![redux](/assets/images/react/redux.gif)

- `Action` state를 바꾸는 방식으로 상태에 변화가 필요할 때 발생된다
- `Reducer` 변화를 일으키는 함수로 Action을 이용해 현재의 state를 어떻게 바꿀지 정의한다. Reducer는 현재의 상태와, 전달 받은 액션을 참고하여 새로운 상태를 만들어서 반환한다.
- `Store` 상태들을 저장하고 있는 중앙 저장소로 현재의 앱 상태, Reducer 그리고 내장 함수 등이 포함되어 있다
- `Dispatch` Store의 내장 함수 중 하나로 Action을 발생시킨다
- `Subscribe` Store의 내장 함수 중 하나로 함수 형태의 값을 파라미터로 받아온다. subscribe 함수에 특정 함수를 전달해주면, Action이 Dispatch 되었을 때 마다 전달해준 함수가 호출된다. ⇒ 아래에서는 리덕스 훅 중 하나인 useSelector를 사용하여 리덕스 스토어의 상태를 구독하기에 따로 사용하지 않는 함수이다.

### 리덕스 특징

- 상태를 전역적으로 관리하기 때문에 어느 컴포넌트에 상태를 둬야할지 고민할 필요가 없게 한다.
- 데이터 흐름을 단방향으로 흐르게 한다.
- 상태는 읽기 전용이다. 상태 관리에서 불변성 유지가 매우 중요하다. (이를 쉽게 관리하기 위해서 아래에서 immer 패키지를 사용한다.)
- 변화를 일으키는 함수, Reducer는 이전 상태와 액션 객체를 파라미터로 받는다. 이전의 상태는 건들이지 않고, 변화를 일으킨 새로운 상태 객체를 만들어서 반환해야 한다. 동일한 인풋이라면 항상 동일한 아웃풋이 있어야 한다.
- 하나의 어플리케이션 안에는 하나의 스토어만 있는 것이 좋다.

### 리덕스 설치

```bash
yarn add redux react-redux redux-thunk redux-logger history@4.10.1 connected-react-router@6.8.0 immer redux-actions
```

- `redux` JavaScript application의 상태를 관리하기 위해 사용
- `redux-thunk` 리덕스를 사용하는 어플리케이션에서 비동기 작업을 처리할 때 이용하는 미들웨어
- `redux-logger` 리덕스의 로그를 출력하는 미들웨어
- `history` thunk 안에서 라우터의 history 객체 사용할 수 있도록 함
- `connected-react-router` 리덕스에서 주소를 변경 및 확인하기 위해 history 객체를 관리하며 필요에 의해 꺼내쓸 수 있는 유용한 라이브러리
- `immer` 불변성 관리를 위한 패키지
- `redux-actions` 액션 생성 함수를 더 짧은 코드로 작성해 편하게 액션을 관리하기 위한 패키지

> 아래 예시의 이해를 돕기 위한 파일 구조

```
├─ src
│  ├─ redux
│  │   ├─ modules
│  │   │  ├─ test.js
│  │   │  ├─ ...
│  │   │
│  │   ├─ configureStore.js
│  │
│  ├─ Pages
│  │   ├─ Home.js
│  │
│  ├─ index.js
│  ├─ App.js
```

## 모듈 만들기

> src/redux/modules/test.js

```jsx
import { createAction, handleActions } from "redux-actions";
import { produce } from "immer";

// actions
const SET_TEST = "SET_TEST";

// action creators
const setTest = createAction(SET_TEST, (isTesting) => ({
  isTesting,
}));

// initialState
const initialState = {
  isTesting: false,
};

// middleware actions
// 여기서는 사실 기능상으로 필요가 없어 아래 형식만 참고용으로 적어놨다
const setTestAction = () => {
  return function (dispatch, getState, { history }) {
  ...
  };
};

// reducer
export default handleActions(
  {
    [SET_TEST]: (state, action) =>
      produce(state, (draft) => {
        draft.isTesting = action.payload.isTesting;
      }),
  },
  initialState,
);

// action creator export
const actionCreators = {
  setTest,
};

export { actionCreators };
```

## 리덕스 스토어 만들기

> src/redux/configureStore.js

```jsx
import { createStore, combineReducers, applyMiddleware, compose } from "redux";
import thunk from "redux-thunk";
import { createBrowserHistory } from "history";
import { connectRouter } from "connected-react-router";

import Test from "./modules/test";

export const history = createBrowserHistory();

// export한 reducer를 모아 root reducer를 만들기
const rootReducer = combineReducers({
  test: Test,
  router: connectRouter(history),
});

// 스토어에 히스토리 넣기
const middlewares = [thunk.withExtraArgument({ history })];

const env = process.env.REACT_APP_TYPE;

// 개발 환경에서는 redux-logger를 이용해 로그 찍기
if (env === "DEV") {
  const { logger } = require("redux-logger");
  middlewares.push(logger);
}

// 크롬 확장 프로그램, redux devTools 사용 설정하기
// redux devTools 다운로드 => https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd
const composeEnhancers =
  typeof window === "object" && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
    ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
    : compose;

// 미들웨어 묶기
const enhancer = composeEnhancers(applyMiddleware(...middlewares));

// root reducer와 미들웨어를 엮어 스토어 만들기
const store = (initialStore) => createStore(rootReducer, enhancer);

export default store();
```

## 스토어 주입하기

- `Provider`를 이용해서 index.js에 주입하기
- **리덕스와 같은 히스토리를 공유**하기 위해 App.js에서 컴포넌트에 history를 주입하기 위해 원래 사용하던 `BrowserRouter`와 `Route` 대신에 `ConnectedRouter`를 사용하기

> src/index.js

```jsx
...
import store from "./redux/configureStore";
import { Provider } from "react-redux";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
...
```

> src/App.js

```jsx
...
import { ConnectedRouter } from "connected-react-router";
import { history } from "../redux/configureStore";
...
function App() {
  return (
    <React.Fragment>
      <Grid>
        <Header></Header>
        <ConnectedRouter history={history}>
          <Route path="/" exact component={Home} />
        </ConnectedRouter>
      </Grid>
    </React.Fragment>
  );
}
...
```

# Redux Hooks

- 다양한 Redux Hooks이 있지만, 컴포넌트에서 리덕스 액션을 직접 실행하고, 스토어에 저장된 데이터를 사용하기 위해서는 아래 2가지의 훅을 사용하면 된다.
- `useDispatch` 컴포넌트 내에서 dispatch를 사용할 때 사용
- `useSelector` 리덕스 스토어의 상태에 접근할 때 사용

## 액션 실행하기

> src/pages/Home.js

```jsx
import React from "react";
import { actionCreators as userActions } from "../redux/modules/user";
import { useDispatch } from "react-redux";

const Home = (props) => {
  const dispatch = useDispatch();

  const handleTest = () => {
    dispatch(userActions.setTest(true));
  };

  return (
    <React.Fragment>
      <button
        onClick={() => {
          handleTest();
        }}
      >
        Test
      </button>
    </React.Fragment>
  );
};

export default Home;
```

## 스토어 데이터 사용하기

> src/pages/Home.js

```jsx
import React from "react";
import { actionCreators as userActions } from "../redux/modules/user";
import { useSelector, useDispatch } from "react-redux";

const Home = (props) => {
  const dispatch = useDispatch();
  const isTesting = useSelector((state) => state.user.isTesting);

  // 리덕스 스토어에 저장된 isTesting 값과 반대되는 값을 다시 저장하기
  const handleTest = () => {
    dispatch(userActions.setTest(!isTesting));
  };

  return (
    <React.Fragment>
      <button
        onClick={() => {
          handleTest();
        }}
      >
        Test
      </button>
    </React.Fragment>
  );
};

export default Home;
```

# Ref.

- [Redux - 자바스크립트 앱을 위한 예측 가능한 상태 컨테이너. ](https://ko.redux.js.org/)

- [mobx vs recoil vs redux](https://www.npmtrends.com/mobx-vs-recoil-vs-redux)

- [Redux vs Mobx vs Recoil](https://ykss.netlify.app/react/redux_mobx_recoil/)

- [[Redux] 08. Redux는 React 라이브러리 중 하나?!](https://lienkooky.tistory.com/72)

- [2. 리덕스의 3가지 규칙](https://react.vlpt.us/redux/02-rules.html)
