# JavaScript 기본

## 변수와 상수

- var는 재선언 가능, let은 재선언 시 SyntaxError
- const는 재할당 불가능, 선언과 동시에 값이 할당되지 않아도 SyntaxError → 이후에 값을 할당할 수 없기 때문

## 자료형과 형 변환

### Date Types

- Primitive Data Type
    - Number, String, Undefined, Null, Boolean
- Non-Primitive Data Type
    - Object, Array, Function
- 자바스크립트는 자료형이 다른 경우 자동으로 형 변환을 수행하는 기능이 있음(Type Casting), **묵시적 형 변환**
- 프로그래머가 parseInt 등을 사용해서 형 변환을 하는 경우 **명시적 형 변환**이라고 부름

## 함수표현식 & 화살표 함수

- 함수 선언식은 호이스팅이 가능, 때문에 함수 표현식을 권장

## 콜백 함수

```jsx
function checkMood(mood, goodCallback, badCallback) {
  if (mood === "good") {
    goodCallback();
  } else {
    badCallback();
  }
}

function cry() {
  console.log("action :: cry");
}

function sing() {
  console.log("action :: sing");
}

function dance() {
  console.log("action :: Dance");
}

checkMood("good", sing, cry);
```