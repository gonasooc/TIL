[React - useState의 경쟁자 useReducer](https://youtu.be/E7bNzWrlKTE?si=WJt3mXDY0W6mD_hm)

- useState
  ```jsx
  import { useState } from "react";
  import "./App.css";

  function App() {
    const [count, setCount] = useState(0);

    const down = () => {
      setCount(count - 1);
    };

    const reset = () => {
      setCount(0);
    };

    const up = () => {
      setCount(count + 1);
    };

    return (
      <>
        <input type="button" value="-" onClick={down} />
        <input type="button" value="0" onClick={reset} />
        <input type="button" value="+" onClick={up} />
        <span>{count}</span>
      </>
    );
  }

  export default App;
  ```
- useReducer
  ```jsx
  import { useReducer, useState } from "react";
  import "./App.css";

  function App() {
    const countReducer = (oldCount, action) => {
      if (action === "UP") {
        return oldCount + 1;
      } else if (action === "DOWN") {
        return oldCount - 1;
      } else if (action === "RESET") {
        return 0;
      }
    };

    // const [count, setCount] = useState(0);
    // useReducer의 첫 번째 parameter는 reducer 함수, 두 번째 parameter는 기본값
    // set 대신에 dispatch라는 이름을 사용
    const [count, countDispatch] = useReducer(countReducer, 0);

    const down = () => {
      // setCount(count - 1);
      // dispatch 직접 값을 주는 게 아니라 action을 전달
      // state를 직접 사용자가 전달하는 게 아니라
      // action값만 주고 그 값의 처리는 별도의 함수가 처리
      countDispatch("DOWN");
    };

    const reset = () => {
      // setCount(0);
      countDispatch("RESET");
    };

    const up = () => {
      // setCount(count + 1);
      countDispatch("UP");
    };

    return (
      <>
        <input type="button" value="-" onClick={down} />
        <input type="button" value="0" onClick={reset} />
        <input type="button" value="+" onClick={up} />
        <span>{count}</span>
      </>
    );
  }

  export default App;
  ```
  ```jsx
  import { useReducer, useState } from "react";
  import "./App.css";

  function App() {
    const [number, setNumber] = useState(1);

    const countReducer = (oldCount, action) => {
      if (action.type === "UP") {
        return oldCount + action.number;
      } else if (action.type === "DOWN") {
        return oldCount - action.number;
      } else if (action.type === "RESET") {
        return 0;
      }
    };

    const [count, countDispatch] = useReducer(countReducer, 0);

    const down = () => {
      countDispatch({ type: "DOWN", number: number });
    };

    const reset = () => {
      countDispatch({ type: "RESET", number: number });
    };

    const up = () => {
      countDispatch({ type: "UP", number: number });
    };

    const changeNumber = (event) => {
      setNumber(Number(event.target.value));
    };

    return (
      <>
        <input type="button" value="-" onClick={down} />
        <input type="button" value="0" onClick={reset} />
        <input type="button" value="+" onClick={up} />
        <input type="text" value={number} onChange={changeNumber} />
        <span>{count}</span>
      </>
    );
  }

  export default App;
  ```
