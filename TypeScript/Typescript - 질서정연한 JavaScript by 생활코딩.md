## 커버 페이지

### JavaScript + 타입 체크 = TypeScript

- TypeScript 실행 환경에 JavaScript를 코딩하면 100% 동작, 하지만 JavaScript 실행 환경에 TypeScript를 코딩하면 동작하지 않음

### 데이터 타입 체크

- JavaScript에서는 변수의 데이터 타입을 명확하게 알기 어렵기 때문에 데이터 타입의 안정성이 낮음
- TypeScript는 변수의 데이터 타입을 명확하게 지정해주어 안정성을 높여줌

### 왜 사용해야 하는가?

- JavaScript에 타입 체크 기능 추가
- 타입스크립트의 문법은 아는만큼, 원하는만큼 사용할 수 있음
- github에 따르면 4번째로 인기있는 언어가 되었음

## TypeScript의 데이터 타입과추론

### 변수에 데이터 타입을 지정하는 방법

- 타입스크립트는 변수 선언 시 데이터 타입을 지정 → 타입스크립트를 자바스크립트로 컴파일 할 때 데이터 타입에 다른 값이 할당되려 할 때 오류를 발생시켜 개발자에게 알려줌 → 이를 통해 코딩할 때 버그를 잡을 수 있음
  ```tsx
  let myname: string = "egoing";
  ```

### 기본 데이터 타입

- `number`: 숫자 타입으로, 정수와 실수를 포함
- `string`: 문자열 타입
- `boolean`: 참(true)과 거짓(false)을 나타내는 불리언 타입
- `null`: 값이 없음을 나타내는 타입
- `undefined`: 값이 할당되지 않은 변수의 기본값인 타입

### \***\*객체 타입\*\***

- `object`: 객체를 나타내는 타입
- `array`: 동일한 타입의 요소를 가진 배열을 나타내는 타입
- `tuple`: 각 요소가 다른 타입을 가질 수 있는 배열을 나타내는 타입 (TS 전용)

### \***\*특수 타입\*\***

- `any`: 어떠한 타입이든 할당될 수 있는 타입 (TS 전용)
- `unknown`: 타입을 미리 알 수 없는 경우에 사용되는 타입. 이 타입은 안전한 타입 검사를 위해 사용 (TS 전용)
- `never`: 절대 발생하지 않는 값의 타입을 나타냄. 예를 들어, 함수가 항상 예외를 발생시키거나 무한 루프를 실행할 때 이 타입을 사용할 수 있음 (TS 전용)

### 타입 추론 기능

- 아래 코드에서 age 변수에 명시적으로 타입을 지정하지 않았지만, 초기 값으로 숫자 30을 할당함 → 이때 타입스크립트는 age 변수의 타입을 자동으로 number로 추론
- 타입 추론은 코드를 간결하게 작성할 수 있도록 도와주지만, 타입 추론이 모호한 경우나 복잡한 로직에서는 타입을 명시해주는 것이 좋음
  ```tsx
  let age = 30;
  ```

## TypeScript의 Array와 Tuple

### TypeScript에서 Array와 Tuple의 데이터 타입

- Array 타입
  ```tsx
  // 첫 번째 방법: 타입 + []
  let arr1: number[] = [1, 2, 3];

  // 두 번째 방법: Array<타입>
  let arr2: Array = [1, 2, 3];
  ```
- Tuple 타입
  - Tuple은 고정된 길이와 타입의 배열 → 각 요소의 타입과 순서가 정해져 있고, 이는 JavaScript에는 없는 데이터 타입이며, TypeScript에서만 제공
    ```tsx
    let tuple: [string, number, boolean] = ["Hello", 42, true];
    ```

## TypeScript의 객체

### TypeScript에서 객체의 데이터 타입

- JavaScript
  ```jsx
  const user = {
    name: "John",
    age: 25,
  };
  ```
- TypeScript
  ```tsx
  const user: { name: string; age: number } = {
    name: "John",
    age: 25,
  };
  ```
- 만약 user 객체에 잘못된 데이터 타입의 속성을 할당하려고 하면, TypeScript는 컴파일 시점에 오류를 발생시킴
  ```tsx
  const user: { name: string; age: number } = {
    name: "John",
    age: "25", // 오류: 타입 'string'은 'number'에 할당할 수 없습니다.
  };
  ```

## TypeScript의 함수

### TypeScript에서 객체의 데이터 타입

- TypeScript에서 함수를 사용할 때, 매개변수와 반환 값에 대한 데이터 타입 지정 가능
  ```tsx
  function add(a: number, b: number): number {
    return a + b;
  }
  ```
- 함수에서 일부 매개변수는 선택적으로 받을 수 있게 만들고 싶을 때, 매개변수 뒤에 '?'를 사용 → 이렇게 하면 해당 매개변수는 선택적이 되어, 값을 전달하지 않아도 됨
  ```tsx
  function greet(name: string, greeting?: string): string {
    if (greeting) {
      return `${greeting}, ${name}!`;
    } else {
      return `Hello, ${name}!`;
    }
  }
  ```

## TypeScript의 Type Aliases

- 원시 데이터 타입의 별칭
  ```tsx
  type Age = number;
  const myAge: Age = 30;
  ```
- Array와 Tuple, 객체, 함수에 적용한 사례
  ```tsx
  // Array
  type Names = string[];
  const myFriends: Names = ["Alice", "Bob", "Charlie"];

  // Tuple
  type Coordinates = [number, number];
  const myLocation: Coordinates = [37.7749, -122.4194];

  // 객체
  type User = {
    id: string;
    name: string;
    age: number;
  };
  const user: User = { id: "1", name: "John Doe", age: 28 };

  // 함수
  type GreetingFunction = (name: string) => string;
  const greet: GreetingFunction = (name) => `Hello, ${name}!`;
  ```
- 좀 더 복잡한 형태
  ```tsx
  type UserID = string;
  type UserName = string;
  type Age = number;

  type User = {
    id: UserID;
    name: UserName;
    age: Age;
  };

  const user: User = { id: "1", name: "John Doe", age: 28 };
  ```
