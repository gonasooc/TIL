## 메모

### React

- Redux → 리덕스는 리액트 생태계에서 가장 사용률이 높은 상태관리 라이브러리
- useState vs Redux → useState는 컴포넌트 단위로 상태를 관리하지만, Redux는 전역으로 데이터 상태를 관리함
- Redux 3가지 규칙
  - 하나의 애플리케이션 안에는 하나의 스토어 → 여러 개의 스토어를 사용하는 것도 가능하진 권하지 않음
  - 상태는 읽기적용 → useState처럼 Redux에서도 기존 상태는 건드리지 않고 새로운 상태를 생성하여 업데이트를 해주는 방식으로 처리, Redux에서 불변성을 유지해야 하는 이유는 내부적으로 데이터가 변경되는 것을 감지하기 위해서
  - 변화를 일으키는 함수, Reducer는 순수한 함수 → Reducer 함수는 이전 상태와 액션 객체를 parameter로 받음 → 이전 상태는 건드리지 않고 변화를 일으킨 새로운 상태 객체를 만들어서 반환
- {React} 리덕스의 기본 흐름
  - 하나의 중앙 데이터 store를 갖습니다. (데이터는 state(상태)를 말합니다.
  1. () ⇒ 상태 값을 변경해야할 때 데이터들의 state 값을 수정해줄 reducer 함수를 만
     듭니다.
  2. dispatch를 사용하여 reducer에게 state 값을 수정하라고 action 값과 함께
     요청
- npm install react-redux
- npm install @reduxjs/toolkit
- npm install redux-logger

## 수업 실습

- https://github.com/udemy-team13/training/tree/main/gonasooc/practice/230703/practice-redux

본 후기는 유데미-스나이퍼팩토리 10주 완성 프로젝트캠프 학습 일지 후기로 작성 되었습니다. #프로젝트캠프 #프로젝트캠프후기 #유데미 #스나이퍼팩토리 #웅진씽크빅 #인사이드아웃 #IT개발캠프 #개발자부트캠프 #리액트 #react #부트캠프 #리액트캠프
