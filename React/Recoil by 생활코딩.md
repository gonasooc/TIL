[Redux의 쉬운 대안 - 페북이 만든 Recoil](https://youtu.be/T4QSK3Un4rU)

### \***\*Redux의 쉬운 대안 - 페북이 만든 Recoil\*\***

- `useState`로 생성된 변수는 해당 함수 컴포넌트의 지역 변수
- props drilling을 방지하기 위한, 불필요한 리랜더링을 막기 위한 전역 관리
- 전역 상태 관리의 별도의 `state`를 만드는데, 그걸 `atom`이라고 명명
- 최상위 `index.js`에서 `<RecoilRoot />`로 감싸고 설정
  ```jsx
  const root = ReactDOM.createRoot(document.getElementById("root"));
  root.render(
    <React.StrictMode>
      <RecoilRoot>
        <App />
      </RecoilRoot>
    </React.StrictMode>
  );
  ```
- 별도의 전역 관리 파일(atom.js)에 전역 관리할 state를 선언
  ```jsx
  import { atom } from "recoil";

  export const countState = atom({
    key: "count",
    default: 10,
  });
  ```
- useRecoilState를 통해 useState처럼 해당 state를 가져옴
  ```jsx
  import { useState } from "react";
  import "./App.css";
  import { useRecoilState } from "recoil";
  import { countState } from "./atom";

  function Counter() {
    const [count, setCount] = useRecoilState(countState);

    return (
      <div>
        <h1>Counter</h1>
        <button
          onClick={() => {
            setCount(count + 1);
          }}
        >
          +
        </button>
        {count}
      </div>
    );
  }

  function DisplayCounter() {
    const [count] = useRecoilState(countState);
    return <div>{count}</div>;
  }

  function App() {
    return (
      <div>
        <Counter />
        <DisplayCounter />
      </div>
    );
  }

  export default App;
  ```

### 추가로 학습 필요한 부분

- Selectors
- Asynchronous Data Queries
- Atom Effects
