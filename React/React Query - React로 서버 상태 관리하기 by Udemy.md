# 섹션 1: 쿼리 생성 및 로딩/에러 상태

## React Query 소개

### React Query

- React Query는 React 앱의 서버 상태를 관리하는 라이브러리(Server State Management for React)

### Client State vs. Server State

- Client state: 웹 브라우저 세션과 관련된 모든 정보 ex) 언어 선택, 다크 모드 등
- Server state: 서버에 저장되는 클라이언트에 표시하는 데에 필요한 데이터 정보 ex) 블로그 게시물 데이터 등

### What problem does React Query solve?

- 서버 데이터 캐시를 관리
- fetch나 axios를 사용해 서버로 바로 이동하지 않고 React Query 캐시를 요청함

### React Query Manages Data

- 서버의 새 데이터로 캐시를 업데이트하는 사용자의 영역
- 별도의 key를 할당한 캐시가 있고 해당 key값을 통해 캐시를 업데이트함

### Plus…

- Loading / Error states → 서버에 대한 모든 쿼리의 로딩 및 오류 상태를 유지해주기 때문에 별도로 관리할 필요 없음
- Pagination / infinite scroll → 사용자를 위해 데이터의 pagination이나 무한 스크롤이 필요한 경우 데이터를 쪼개서 가져올 수 있는 도구 제공
- Prefetching → 사용자의 행동을 예측해서 Prefetching이 가능함
- Mutations → 데이터의 변이(Mutations)나 업데이트를 관리할 수 있음
- De-duplication of requests → 쿼리는 키로 식별되기 때문에 쿼리를 한번에 보낼 수 있음
- Retry on error → 재시도 관리
- Callback → 쿼리가 성공하거나 오류가 났을 때를 구별해서 조치를 취할 수 있도록 콜백 관리 가능

## 첫 번째 프로젝트: Blog-em Ipsum

### First Project! Blog-em Ipsum

- Get data from https://jsonplaceholder.typicode.com/
- Very simple, focus on React Query concepts
  - Fetching data
  - Loading / error states
  - React Query dev tools
  - Pagination
  - Prefetching → pagination에 prefetching을 적용하면 다음 페이지 데이터를 미리 가져왔을 때 사용자의 페이지 로딩을 줄여줄 수 있음
  - Mutations

### Getting Started

- `npm install react-query`
- Create query client
  - 쿼리와 서버의 데이터 캐시를 관리하는 클라이언트(Client that manages queries and cache)
- Apply QueryProvider
  - 자녀 컴포넌트에 캐시와 클라이언트 구성을 제공할 QueryProvider(Provides cache and client config to children)
  - 이에 대한 값으로 사용될 쿼리 클라이언트도 필요(Takes query client as the value)
- useQuery 훅 실행(Run useQuery)
  - 서버에서 데이터를 가져오는 훅(Hook that queries the server)

## useQuery로 쿼리 생성하기

- react-query에서 QueryClient와 QueryClientProvider가 필요
- 클라이언트가 가지고 있는 캐시와 모든 기본 옵션을 Provider의 하위 컴포넌트로 사용할 수 있음, react-query hook 사용 가능
- App.jsx

  ```jsx
  import { Posts } from "./Posts";
  import "./App.css";

  import { QueryClient, QueryClientProvider } from "react-query";

  const queryClient = new QueryClient();

  function App() {
    return (
      // provide React Query client to App
      <QueryClientProvider client={queryClient}>
        <div className="App">
          <h1>Blog Posts</h1>
          <Posts />
        </div>
      </QueryClientProvider>
    );
  }

  export default App;
  ```

- 구조 분해 할당을 통해 useQuery를 담아줄 수 있음, useQuery는 다양한 속성을 가진 개체를 반환함
- useQuery는 몇 가지 인수를 사용함
  - 쿼리 키를 사용
  - 그런 다음 쿼리 함수를 사용 → 데이터를 가져오는 비동기 함수여야 함
    ```jsx
    // replace with useQuery
    const { data } = useQuery("posts", fetchPosts);
    ```

## 로딩 상태와 에러 상태 처리하기

- useQuery
  https://tanstack.com/query/v4/docs/react/reference/useQuery
- isLoading, isError를 통해 분기 처리 가능
- isLoading과 isFetching의 차이
  - isFetching - 비동기 쿼리가 해결되지 않았음을 의미(the async query function hasn’t yet resolved
  - isLoading - 가져오는 상태를 의미, 표시할 캐시 데이터 없음(no cached data, plus isFetching)
  - 캐시된 데이터가 있을 때와 없을 때를 구분할 때 필요
- react-query는 기본적으로 세 번 시도한 후에 해당 데이터를 가져올 수 없다고 판단하여 isError를 반환함(횟수 변경 가능)

  ```jsx
  // replace with useQuery
  const { data, isLoading, isError, error } = useQuery("posts", fetchPosts);

  if (isLoading) return <h3>Loading...</h3>;
  if (isError)
    return (
      <>
        <h3>Oops, something went wrong</h3>
        <p>{error.toString()}</p>
      </>
    );
  ```

## React Query 개발자 도구

- React Query Dev Tools
  - Shows queries (by key) - 쿼리 키로 쿼리 표시
    - status of queries
    - last updated timestamp
  - Data explorer
  - Query explorer - 쿼리 탐색기
- import 하기

  ```jsx
  import { Posts } from "./Posts";
  import "./App.css";

  import { QueryClient, QueryClientProvider } from "react-query";
  import { ReactQueryDevtools } from "react-query/devtools";

  const queryClient = new QueryClient();

  function App() {
    return (
      // provide React Query client to App
      <QueryClientProvider client={queryClient}>
        <div className="App">
          <h1>Blog Posts</h1>
          <Posts />
        </div>
        <ReactQueryDevtools />
      </QueryClientProvider>
    );
  }

  export default App;
  ```

## staleTime vs cacheTime

- Stale Data
  - React Query에서 데이터가 만료됐다는 건 무슨 뜻일까요? (Why does it matter if the data is stale?)
  - 데이터 리페칭(refetching)은 만료된 데이터에서만 실행(Data refetch only triggers for stale data)
    - For example, component remount, window refocus
    - staleTime은 데이터를 허용하는 ‘최대 나이’라고 할 수 있음(`staleTime` translates to “max age”
  - 데이터가 만료됐다고 판단하기 전까지 허용하는 시간이 staleTime (How to tolerate data potentially being out of date?) - ex) 웹사이트에 표시된 데이터가 10초까지는 그대로여도 괜찮다면 10초로 설정, 데이터의 성격에 따라 달라진다고 볼 수 있음
    ```jsx
    const { data, isLoading, isError, error } = useQuery("posts", fetchPosts, {
      staleTime: 2000,
    });
    ```
- staleTime vs. cacheTime
  - `staleTime` is for re-fetching
  - Cache is for data that might be re-used later
    - query goes into “cold storage” if there’s no active `useQuery`
    - cache data expires after `cacheTime` (default: five minutes)
      - how long it’s been since the last active `useQuery`
    - After the cache expires, the data is garbage collected - 캐시가 만료되면 가비지 컬렉션이 실행되고 클라이언트 데이터는 사용할 수 없음
  - Cache is backup data to display while fetching - 데이터가 캐시에 있는 동안에는 페칭(fetching)할 때 사용될 수 있음

# 섹션 2: 페이지 매김, 프리페칭과 변이

## 코드 퀴즈! 블로그 댓글을 위한 쿼리 생성하기

- `post.id`를 받아와야 하기 때문에 익명함수로 작성(인자가 필요한 함수인 경우 익명함수를 통해 함수를 실행시켜야 함)

  ```jsx
  const { data, isLoading, isError, error } = useQuery("comments", () =>
    fetchComments(post.id)
  );

  if (isLoading) return <h3>Loading...</h3>;

  if (isError) {
    return (
      <>
        <h3>Error</h3>
        <p>{error.toString()}</p>
      </>
    );
  }
  ```

- 위와 같은 이전과 같은 방법으로 데이터를 fetching 하면 다른 post.id를 넣어도 query key는 같기 때문에 이전에 만들어진 cache data만 불러오게 됨
- \*참고하면 좋을 블로그
  - [https://velog.io/@cloud_oort/React-Query-공부-2](https://velog.io/@cloud_oort/React-Query-%EA%B3%B5%EB%B6%80-2)
    - 이 배열은 우리가 흔히 알고 있는 useEffect에서 사용하는 dependency array와 같은 역할을 할 것이다.
      ```jsx
      const { data } = useQuery(["comments", post.id], () =>
        fetchComments(post.id)
      );
      ```

## 쿼리 키

- Why don’t comments refresh?
  - 모든 쿼리가 `comment` 쿼리 키를 동일하게 사용하기 때문 - Every query uses the same key (`comments`)
  - 어떠한 트리거가 있어야 함 - Data for queries with known keys only refreshed upon trigger
  - Example triggers:
    - compoment remount
    - window refocus
    - running refetch function
    - automated refetch
    - query invalidation after a mutation
- Solution?
  - Option: remove programmatically for every new title
    - it’s not easy
    - it’s not really what we want
  - No reason to remove data from the cache
    - we’re not even performing the same query!
  - 해당 쿼리는 게시물 ID를 포함 - Query includes post ID
    - 쿼리별로 캐시를 남길 수 있음 - Chche on a per-query basis
    - “comments” 쿼리에 대한 캐시를 공유하지 않아도 됨 - don’t share cache for any “comments’ query regardless of post id
  - 각 게시물에 대한 쿼리에 라벨을 설정하면 됨 - What we really want: label the query for each post separately
- Array as Query Key
  - 바로 쿼리 키에 문자열(String) 대신 배열(Array)을 전달하면 가능 - Pass array for the query key, not just a string
  - Treat the query key as a _dependency array_
    - What key changes, create a new query - 의존성 배열에 다르면 완전히 다른 것으로 간주됨
  - 데이터를 가져올 때 사용하는 쿼리 함수에 있는 값이 쿼리 키에 포함되어야 함 - Query function values should be part of the key

```
// replace with useQuery
  const { data, isLoading, isError, error } = useQuery(["comments", post.id], () =>
    fetchComments(post.id)
  );
```

## 페이지 매김(Pagination)

- Pagination
  - Track current page in component state (`currentPage`)
  - Use query keys that include the page number `[”post”, currentPage]`
  - User clicks “next pages” or “previous page” button
    - update `currentPage` state
    - fire off new query
- prefetching 이전
  ```jsx
  import { useState } from "react";
  import { useQuery } from "react-query";

  import { PostDetail } from "./PostDetail";
  const maxPostPage = 10;

  async function fetchPosts(pageNum) {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${pageNum}`
    );
    return response.json();
  }

  export function Posts() {
    const [currentPage, setCurrentPage] = useState(1);
    const [selectedPost, setSelectedPost] = useState(null);

    // replace with useQuery
    const { data, isLoading, isError, error } = useQuery(
      ["posts", currentPage],
      () => fetchPosts(currentPage),
      {
        staleTime: 2000,
      }
    );

    if (isLoading) return <h3>Loading...</h3>;
    if (isError)
      return (
        <>
          <h3>Oops, something went wrong</h3>
          <p>{error.toString()}</p>
        </>
      );

    return (
      <>
        <ul>
          {data.map((post) => (
            <li
              key={post.id}
              className="post-title"
              onClick={() => setSelectedPost(post)}
            >
              {post.title}
            </li>
          ))}
        </ul>
        <div className="pages">
          <button
            disabled={currentPage <= 1}
            onClick={() => {
              setCurrentPage((previousValue) => previousValue - 1);
            }}
          >
            Previous page
          </button>
          <span>Page {currentPage}</span>
          <button
            disabled={currentPage >= maxPostPage}
            onClick={() => {
              setCurrentPage((previousValue) => previousValue + 1);
            }}
          >
            Next page
          </button>
        </div>
        <hr />
        {selectedPost && <PostDetail post={selectedPost} />}
      </>
    );
  }
  ```

## 데이터 프리페칭(Pre-fetching)

- Prefetching

  - Prefetch
    - 데이터를 캐시에 추가 - adds data to cache
    - 구성 가능하나 기본값으로는 만료(stale) 상태 - automatically stale (configurable)
    - shows while re-fetching
      - 캐시가 만료되지 않았다는 가정 하에 - as long as cache hasn’t expired!
  - Prefetching can be used for any anticipated data needs
    - not just pagination!

- prefetching 적용
  - prefetchQuery는 queryClient에 있음 → useQueryClient 호출
  - useQuery와 작성 방식 비슷함
    ```jsx
    import { useEffect, useState } from "react";
    import { useQuery, useQueryClient } from "react-query";

    import { PostDetail } from "./PostDetail";
    const maxPostPage = 10;

    async function fetchPosts(pageNum) {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts?_limit=10&_page=${pageNum}`
      );
      return response.json();
    }

    export function Posts() {
      const [currentPage, setCurrentPage] = useState(1);
      const [selectedPost, setSelectedPost] = useState(null);

      const queryClient = useQueryClient();

      useEffect(() => {
        if (currentPage < maxPostPage) {
          const nextPage = currentPage + 1;
          queryClient.prefetchQuery(["posts", nextPage], () =>
            fetchPosts(nextPage)
          );
        }
      }, [currentPage, queryClient]);

      // replace with useQuery
      const { data, isLoading, isError, error } = useQuery(
        ["posts", currentPage],
        () => fetchPosts(currentPage),
        {
          staleTime: 2000,
          keepPreviousData: true,
        }
      );

      if (isLoading) return <h3>Loading...</h3>;
      if (isError)
        return (
          <>
            <h3>Oops, something went wrong</h3>
            <p>{error.toString()}</p>
          </>
        );

      return (
    		...
      );
    }
    ```

## isLoading vs isFetching

- `isFetching`
  - async query function이 해결되지 않았을 때 참에 해당함, 아직 데이터를 가져오는 중 - the async query fuction hasn’t yet resolved
- `isLoading`
  - 캐시된 데이터가 없고 데이터를 가져오는 상황에 해당, `isFetching`의 부분 집합 - no cached data, plus `isFetching`

## Mutations

- Mutations
  - 변이는 서버에 데이터를 업데이트하도록 서버에 네트워크 호출을 실시 - Mutations: making a network call that changes data on the server
