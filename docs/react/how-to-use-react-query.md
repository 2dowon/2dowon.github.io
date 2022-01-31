---
layout: default
title: React Query 사용법
date: 2021-12-04 23:12:35
parent: React
nav_order: 5
---

# React Query 사용법

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

기존에 진행하던 프로젝트에서는 React Query를 써본 적이 없었는데, 이번에 사용하게 될 주 스택 중 하나가 React Query라서 요새 조금씩 써보고 있는 중이다. 공부하는 김에 아래에서 가볍게 정리해두려고 한다.

# React Query

- React를 위한 데이터를 fetch할 수 있는 라이브러리
- server state를 fetch, caching, synchronizing(동기화), update할 수 있다.

## React Query를 왜 사용할까 🤔

- 기존 방법보다 훨씬 좋고, 편리한 방식으로 데이터를 fetch 시킬 수 있다.
- 데이터를 준비하고, state에 넣고, loading도 따로 만들어서 false로 두는 이 모든 과정을 하지 않아도 된다.
- React Query는 데이터를 캐시에 저장해두기 때문에 한번 불러온 데이터를 다시 보는 페이지로 가게 되더라도 다시 부르지 않는다. (로딩이 또 보이지 않는다. 캐시에 저장해둔 데이터를 보여주면 되기 때문)

## 일반적인 data fetch

- API를 통해 data를 가져오기 위해서 [jsonplaceholder](https://jsonplaceholder.typicode.com/)를 이용
- 데이터를 불러오는 중인지, 데이터를 다 불러왔는지를 확인하기 위해서 `useState` 와 `useEffect` 훅을 통해 확인
- 최신 여부를 보장할 수 없으며 항상 갱신이 필요

![react-query1](/assets/images/react/react-query1.png)

```jsx
import React, { useState, useEffect } from "react";
import axios from "axios";

function App() {
  const [isLoading, setLoading] = useState(false);
  const [isError, setError] = useState(false);
  const [data, setData] = useState({});

  const fetchData = async () => {
    setError(false);
    setLoading(true);

    try {
      const response = await axios(
        "https://jsonplaceholder.typicode.com/posts/1"
      );
      const data = JSON.stringify(response.data);
      setData(data);
    } catch (error) {
      setError(true);
    }
    setLoading(false);
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className="App">
      <h1>React Query</h1>
      {isError && <div>Error</div>}
      {isLoading ? <div>Loading</div> : <div>{data}</div>}
    </div>
  );
}
export default App;
```

## React Query 사용하기

- React Query 설치
  ```tsx
  npm i react-query
  yarn add react-query
  ```

### React Query를 사용하기 전 세팅

> index.js

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { QueryClient, QueryClientProvider } from "react-query";
import App from "./App";

const queryClient = new QueryClient();

ReactDOM.render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

- App 컴포넌트를 `QueryClientProvider`로 감싸줘야 한다
- `QueryClientProvider` 은 client prop을 필요로 한다

### useQuery ⇒ 데이터 읽기 (R)

useQuery가 가질 수 있는 3개의 인자

1. 고유한 key
2. fetcher 함수
3. 선택적인 object

> EX. 5초마다 자동으로 데이터 fetch

```tsx
const { isLoading, data } = useQuery(
  ["fetchData", postId],
  () => fetchData(postId),
  {
    refetchInterval: 5000,
  }
);
```

- 1번째 인자 = 고유한 key ⇒ `"fetchData"`
- 2번째 인자 = fetcher 함수 ⇒ `() => fetchData(postId)`
- 3번째 인자 = 선택적인 object ⇒ `{ refetchInterval: 5000, }`
- ✅ 이 방법을 이용하면 주기적으로 백그라운드에서 앱을 업데이트할 수 있기 때문에 좋다

> App.jsx (일반적인 방법의 data fetch를 React Query를 이용한 방법으로 수정)

```jsx
import React from "react";
import axios from "axios";
import { useQuery } from "react-query";

function App() {
  const fetchData = async () => {
    const { data } = await axios(
      "https://jsonplaceholder.typicode.com/posts/1"
    );
    return JSON.stringify(data);
  };

  const { isLoading, error, data } = useQuery("fetchData", fetchData);

  return (
    <>
      <div className="App">
        <h1>React Query</h1>
        {error && <div>Error</div>}
        {isLoading ? <div>Loading</div> : <div>{data}</div>}
      </div>
    </>
  );
}
export default App;
```

- `useQuery`라는 hook이 fetcher함수 fetchCoins를 불러오고, fetcher 함수가 `isLoading`이라면 그 값을 true로 말해줄 것이다.
- 데이터를 fetch하는 함수인 fetcher 함수는 fetch promise인 JSON 데이터를 리턴해야 한다.
- isLoading이 false로 즉, 데이터를 다 불러오게 되면 그 데이터를 `data`에 넣어준다.
- 만약 많은 useQuery를 사용한다면 isLoading, data와 같은 다양한 값에 별칭을 줄 수 있다.
  ```jsx
  const { isLoading: dataLoading, data: fetchData } = useQuery(
    "fetchData",
    fetchData
  );
  ```
  ⇒ 위 코드에서 fetchData의 isLoading은 dataLoading 이름으로, data는 fetchData라는 이름으로 구분해서 사용할 수 있다.
- 만약 fetcher 함수에 인자가 필요한 경우, `useQuery([고유 ID, 인자], () ⇒ 함수이름(인자));` 를 hook 자리에 넣어준다

  ```jsx
  const fetchData = async (postId) => {
  	const { data } = await axios(
  		`https://jsonplaceholder.typicode.com/posts/${postId}`
  	);
  	return JSON.stringify(data);
  };

  const postId = 1;
  const { isLoading: dataLoading, data:fetchData } = = useQuery(["fetchData", postId], () => fetchData(postId));
  ```

### useMutation ⇒ 데이터 생성, 업데이트, 삭제 (CUD)

- 데이터를 업데이트하기 위한 비동기 함수를 인자로 갖고, mutation을 실행하기 위한 mutate 함수를 반환
- `onSuccess`를 통해서 요청이 성공적으로 끝난 후의 일들을 정의
  ⇒ 보통 여기서 `queryCache.invalidateQueries('')` 를 통해서 데이터를 불러오는 useQuery의 캐시를 무효화시켜서 React Query가 데이터를 다시 불러오도록 만들 수 있다

![react-query2](/assets/images/react/react-query2.png)

```jsx
import React, { useState } from "react";
import axios from "axios";
import { useMutation } from "react-query";

function App() {
  const [title, setTitle] = useState("");
  const [body, setBody] = useState("");

  const mutatePost = useMutation(
    () =>
      axios.post(
        "https://jsonplaceholder.typicode.com/posts",
        { title, body, userId: new Date() },
        {
          headers: {
            "Content-type": "application/json; charset=UTF-8",
          },
        }
      ),
    {
      onSuccess: () => {
        // post가 제대로 되고 있는지 확인
        // console.log(mutatePost.data.data);
        // {title: 'hello', body: 'react-query', userId: "2021-12-02T09:57:50.393Z" }
        setTitle("");
        setBody("");
      },
    }
  );

  return (
    <>
      <div className="App">
        <h1>React Query</h1>
        <form
          onSubmit={(e) => {
            e.preventDefault();
            mutatePost.mutate();
          }}
        >
          <input
            type="text"
            placeholder="title"
            onChange={(event) => setTitle(event.target.value)}
            value={title}
          />
          <input
            type="text"
            placeholder="body"
            onChange={(event) => setBody(event.target.value)}
            value={body}
          />
          <button>Create</button>
        </form>
      </div>
      <ReactQueryDevtools initialIsOpen={true} />
    </>
  );
}
export default App;
```

### queryCache

- 데이터를 불러오는 useQuery는 아까 5초마다 fetch하는 예시처럼 refetchInterval 옵션을 설정하면 자동으로 데이터를 불러와서 갱신하지만, 이와 같은 옵션을 설정하지 않는다면 기본적으로는 시간이 지나면 캐싱된 데이터는 최신의 상태가 아니게 된다.
- 따라서 강제적으로 데이터를 새로 가져오도록 해야되는 경우에는 `queryClient.invalidateQueries()` 를 이용하면 쿼리를 무효화할 수 있다. 이때 인자를 아무것도 넘기지 않으면 캐시에 있는 모든 쿼리를 무효화하고, 쿼리의 고유 id값을 전달하면 해당 쿼리만 무효화할 수 있다.

  ```jsx
  // 캐시에 있는 모든 쿼리를 무효화
  queryClient.invalidateQueries();

  // todo로 시작하는 모든 쿼리를 무효화 ex) ['todos', 1], ['todos', 2], ...
  queryClient.invalidateQueries("todos");

  // ['todos', 1] 키를 가진 쿼리를 무효화
  queryClient.invalidateQueries(["todos", 1]);
  ```

<br/>

# Ref.

- [React Query](https://react-query.tanstack.com/)

- [[번역] : React Query - 왜, 그리고 어떻게 사용할 수 있을까?](https://merrily-code.tistory.com/76)

- [React Query로 서버 상태 관리하기](https://blog.rhostem.com/posts/2021-02-01T00:00:00.000Z)
