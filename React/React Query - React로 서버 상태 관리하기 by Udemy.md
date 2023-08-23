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
