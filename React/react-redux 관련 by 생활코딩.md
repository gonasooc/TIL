[react-redux (2022년 개정판)](https://youtu.be/yjuwpf7VH74)

- redux로 관리하는 컴포넌트의 parents는 다시 랜더링되지 않음(값이 변하는 컴포넌트만 랜더링 → 성능면에서도 좋음)
- useSelector는 함수를 인자로 받음 → 물론 arrow function으로 심플하게 적을 수 있음 `const state = useSelector(state ⇒ state.value);`

[Redux toolkit](https://youtu.be/9wrHxqI6zuM)

- Redux는 상태관리자 → React와 무관하게 JavaScript로 프로젝트면 어디서든 쓸 수 있음 ex) createStore, subscribe, getStore, dispatch
- React Redux → Redux를 React에서 쓰려고 하면 까다로움, 그걸 해결해주는 도구 ex) connect, useDispatch, useSelector
- Redux toolkit → configureStore, createSlice, createAsyncThunk
- Redux toolkit의 등장 이유
  - 설정할 것이 너무 많다
  - 미들웨어 설치
  - 반복되는 코드
  - 불변성 유지의 어려움
- store안에 각각의 기능별로 slice를 만들면 RTK가 관리해줌
- 작은 slice들을 모아서 store로 만들 때는 configureStore를 씀
- configureStore 안에는 객체를 전달

```tsx
import * as React from "react";
// import './style.css';
import { createStore } from "redux";
import { Provider, useDispatch, useSelector } from "react-redux";
import { createSlice, configureStore } from "@reduxjs/toolkit";
const counterSlice = createSlice({
  name: "counterSlice",
  initialState: { value: 0 },
  reducers: {
    up: (state, action) => {
      state.value = state.value + action.payload;
    },
  },
});
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
});
// function reducer(state, action) {
//   if (action.type === 'up') {
//     return { ...state, value: state.value + action.step };
//   }

//   return state;
// }
// const initialState = { value: 0 };
// var store = createStore(reducer, initialState);

function Counter() {
  const dispatch = useDispatch();
  const count = useSelector((state) => {
    console.log(state.counter.value);
    return state.counter.value;
  });

  return (
    <div>
      <button
        onClick={() => {
          // dispatch({ type: 'counterSlice/up', step: 2 });
          dispatch(counterSlice.actions.up(2));
        }}
      >
        +
      </button>{" "}
      {count}
    </div>
  );
}

export default function App() {
  return (
    <div>
      <Provider store={store}>
        <Counter />
      </Provider>
    </div>
  );
}
```

[redux toolkit - thunk 를 이용해서 비동기 작업을 처리하는 방법](https://youtu.be/K-3sBc2pUJ4)

- createAsyncThunk는 비동기 작업을 처리하는 action을 만들어준다.
- reducers - 동기 작업은 action을 자동으로 생성해줌 / extraReducers - 비동기 작업은 action create를 자동으로 하지 못함
