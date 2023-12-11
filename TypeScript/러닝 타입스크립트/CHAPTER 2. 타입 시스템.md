# PART 1. 개념

## CHAPTER 2. 타입 시스템

### 타입의 종류

- ‘타입’은 자바스크립트에서 다루는 값의 형태, 즉 `typeof` 연산자가 설명하는 것, 그것을 의미합니다.
- 자바스크립트의 일곱 가지 기본 원시 타입(primitive type)이 타입스크립트의 기본적인 타입입니다.
  - `null`
  - `undefined`
  - `boolean` // ture 혹은 false
  - `string` // “”, “Hi!”, “abc123”, …
  - `number` // 0, 2.1, -4, …
  - `bigint` // 0n, 2n, -4n, …
  - `symbol` // Symbol(), Symbol(”hi”), …
- 타입스크립트의 순서

    ```tsx
    let firstName = "Whitney";
    firstName.length();
    
    // This expression is not callable.
    // Type 'Number' has no call signatures.
    ```

  - 코드를 읽고 `firstName`이라는 변수를 이해 → 초깃값이 “Whitney”이므로 `firstName`이 `string`이라고 결론 지음 → `firstName`의 .length 멤버를 함수처럼 호출하는 코드 확인 → `string`의 .length 멤버는 함수가 아닌 숫자라는 오류를 표시, 즉 함수처럼 호출할 수 없음
- 오류 종류
  - 구문 오류: 타입스크립트가 자바스크립트로 변환되는 것을 차단한 경우
  - 타입 오류: 타입 검사기에 따라 일치하지 않는 것이 감지된 경우

### 할당 가능성

- 변수의 초깃값을 기준으로 새롭게 할당된 값읠 타입이 변수의 타입과 동일하지 확인합니다. 타입 스크립트에서 함수 호출이나 변수에 값을 제공할 수 있는지 여부를 확인하는 것을 **할당 가능성(assignability)**이라고 합니다. 즉, 전달된 값이 예상된 타입으로 할당 가능한지 여부를 확인합니다.
- ‘Type…is not assignable to type…’

    ```tsx
    let firstName: string = 'Choi';
    firstName = 123;
    
    // Type 'number' is not assignable to type 'string'.
    ```

  - 첫 번째 type은 할당하려 시도한 값, 두 번째 type은 이미 할당된 값

### 타입 에너테이션

- 변수에 초깃값이 없는 경우, 나중에 사용할 변수의 초기 타입을 파악하려고 시도하지 않고, 기본적으로 변수의 암묵적인 `any` 타입으로 간주합니다. 초기 타입을 유추할 수 없는 변수는 **진화하는 any**라고 부릅니다. 특정 타입을 강제하는 대신 새로운 값이 할당될 때마다 변수 타입에 대한 이해를 발전(진화)시킵니다.
- 일반적으로 `any` 타입으로 진화하는 것을 허용하게 되면 타입 검사 목적을 부분적으로 쓸모없게 만듭니다. (단, 해당 타입에 걸맞는 내장 함수 등은 체크 가능)
- 타입스크립트는 초깃값을 할당하지 않고도 변수의 타입을 선언할 수 있는 구문인 **타입 에너테이션(type annotation)**을 제공합니다.

    ```tsx
    let rocker: string;
    rocker = "Joan Jett";
    ```

- 자바스크립트로 컴파일하면 해당 타입 애너테이션 코드는 삭제되고, 런타임 코드에도 영향을 주지 않습니다.

### 타입 형태

- 타입스크립트는 해당 자료형의 내장 함수의 존재 유무를 체크하거나, 해당 객체의 key값의 존재 유무를 체크하는 등 원래 타입과 일치하는지 확인하는 것 이상을 수행합니다.
- 스크립트 파일과는 다른 ECMA스크립트 모듈 파일의 선언 스코프

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)