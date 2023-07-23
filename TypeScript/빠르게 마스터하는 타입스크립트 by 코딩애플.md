# Part 1 : 꼭 알아야 할 내용

## Typescript 필수문법 10분 정리와 설치 셋팅 (Vue, React 포함)

[타입스크립트 쓰는 이유 & 필수 문법 10분 정리](https://www.youtube.com/watch?v=xkpcNolC270)

### 타입스크립트 쓰는 이유

- JavaScript는 Dynamic Typing 가능 → 원래 숫자 - 숫자만 가능하지만 JS가 알아서 숫자로 바꿔줌(프로젝트가 크면 단점이 되어버림)
- TypeScript는 타입 엄격히 체크해줌
- 엄격한 타입 체크 덕분에 TypeScript는 에러메세지 퀄리티가 좋음

### 타입스크립트 세팅

- `npm i -g typescript`
- index.ts 파일 만들고 작성하되 js 파일로 컴파일해줘야 함 → 터미널에 `tsc -w` 쳐놓으면 자동 변환해서 js파일로 뿌려줌 (typescript watch)
  - 컴파일 옵션은 tsconfig.json에서 세팅

### 타입스크립트 문법

- 변수 타입 지정
  ```tsx
  let 이름: string = "kim";
  이름 = 123; // error

  let 이름2: string[] = [123, "kim"]; // error
  let 이름3: { name?: string } = { name: 123 }; // error
  ```
- 다양한 타입이 들어올 수 있게 하려면 Union type
  ```jsx
  let 이름: string | number = 123;
  ```
- 타입은 변수에 담아쓸 수 있음 Type alias
  ```tsx
  type NameType = string | number;
  let 이름: NameType = 123;
  ```
  ```tsx
  // 이 함수는 파라미터로 number, return 값으로 number가 되어야 하는 설정

  function 함수(x: number): number {
    return x * 2;
  }

  함수("123"); // error
  ```
- array에 쓸 수 있는 tuple 타입
  ```tsx
  type Member = [number, boolean];
  let john: Member = ["123", true]; // error
  ```
- object에 타입 지정해야 할 속성이 너무 많으면
  ```tsx
  type Member = {
    [key: string]: string;
  };

  let john: Member = { name: "kim", age: 123 }; // error
  ```
- class 타입 지정 가능
  ```tsx
  class User {
    name: string;
    constructor(name: string) {
      this.name = name;
    }
  }
  ```

## Typescript 컴파일시 세부설정 (tsconfig.json)

### tsconfig에 들어갈 기타 항목들

```json
{
  "compilerOptions": {
    "target": "es5", // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
    "module": "commonjs", //무슨 import 문법 쓸건지 'commonjs', 'amd', 'es2015', 'esnext'
    "allowJs": true, // js 파일들 ts에서 import해서 쓸 수 있는지
    "checkJs": true, // 일반 js 파일에서도 에러체크 여부
    "jsx": "preserve", // tsx 파일을 jsx로 어떻게 컴파일할 것인지 'preserve', 'react-native', 'react'
    "declaration": true, //컴파일시 .d.ts 파일도 자동으로 함께생성 (현재쓰는 모든 타입이 정의된 파일)
    "outFile": "./", //모든 ts파일을 js파일 하나로 컴파일해줌 (module이 none, amd, system일 때만 가능)
    "outDir": "./", //js파일 아웃풋 경로바꾸기
    "rootDir": "./", //루트경로 바꾸기 (js 파일 아웃풋 경로에 영향줌)
    "removeComments": true, //컴파일시 주석제거

    "strict": true, //strict 관련, noimplicit 어쩌구 관련 모드 전부 켜기
    "noImplicitAny": true, //any타입 금지 여부
    "strictNullChecks": true, //null, undefined 타입에 이상한 짓 할시 에러내기
    "strictFunctionTypes": true, //함수파라미터 타입체크 강하게
    "strictPropertyInitialization": true, //class constructor 작성시 타입체크 강하게
    "noImplicitThis": true, //this 키워드가 any 타입일 경우 에러내기
    "alwaysStrict": true, //자바스크립트 "use strict" 모드 켜기

    "noUnusedLocals": true, //쓰지않는 지역변수 있으면 에러내기
    "noUnusedParameters": true, //쓰지않는 파라미터 있으면 에러내기
    "noImplicitReturns": true, //함수에서 return 빼먹으면 에러내기
    "noFallthroughCasesInSwitch": true //switch문 이상하면 에러내기
  }
}
```

https://www.typescriptlang.org/tsconfig

## 타입스크립트 기본 타입 정리 (primitive types)

```tsx
let 이름: string = "kim";
let 나이: number = 50;
let 결혼했니: undefined = undefined;
```

```tsx
let 회원들: string[] = ["kim", "park"];
```

```tsx
let 회원들: { member1: string; member2: string } = {
  member1: "kim",
  member2: "park",
};
```

- 타입 지정 원래 자동으로 됨 → 타입 지정 문법 생략 가능

## 타입을 미리 정하기 애매할 때 (union type, any, unknown)

- Union Type(타입 2개 이상을 합친 새로운 타입 만들기)
  ```tsx
  let 회원: number | string | boolean = "kim";
  let 회원들: (number | string)[] = [1, "2", 3];
  let 오브젝트: { a: string | number } = { a: "123" };
  ```
- any 타입 → any 타입은 엄밀히 말해서 선언용으로 쓴다기보다는 타입 쉴드 해제 문법
  ```tsx
  let 이름: any;
  이름 = 123;
  이름 = false;
  ```
- unknown 타입 → any보다 안전한 이유는 strict한 타입에 해당 데이터를 넣을 때 error 뱉어줌 → 간단한 수학연산조차도 타입이 맞아야 함, unkown 타입과 number로 연산 불가능
  ```tsx
  let 이름: unknown;
  이름 = 123;
  이름 = {};

  let 변수1: string = 이름; // error
  ```
  ```tsx
  let 나이: string | number;
  나이 + 1; // error, union 타입과 number는 연산이 불가능
  ```
- 타입 선언 예시
  ```tsx
  let user: string = "kim";
  let age: undefined | number = undefined;
  let married: boolean = false;
  let 철수: (string | number | undefined | boolean)[] = [user, age, married];
  ```
  ```tsx
  let 학교: 학교 = {
    score: [100, 97, 84],
    teacher: "Phil",
    friend: "John",
  };
  학교.score[4] = false;
  학교.friend = ["Lee", 학교.teacher];

  type 학교 = {
    score: (number | boolean)[];
    teacher: string;
    friend: string | string[];
  };
  ```

## 함수에 타입 지정하는 법 & void 타입

- void는 실수로 뭔가 return하는 걸 사전에 막을 수 있음
  ```tsx
  function 함수(x: number): void {
    return x + 1; // error
  }

  함수(2);
  ```
- 값이 있을 수도 있고 없을 수도 있는
  ```tsx
  const hello = (name?: string) => {
    name ? console.log(`안녕하세요 ${name}`) : console.log("이름이 없습니다");
  };

  hello();
  ```

## 타입 확정하기 Narrowing & Assertion

- type이 아직 하나로 확정되지 않았을 경우 Type Narrowing
  ```tsx
  // before

  function 내함수(x: number | string) {
    return x + 1;
  }

  내함수(123);
  ```
  ```tsx
  // after

  function 내함수(x: number | string) {
    if (typeof x === "number") {
      console.log(x + 1);
    } else {
      console.log("숫자를 입력하세요");
    }
  }

  내함수("123");
  ```
- else문도 확실하게 넣어주는 편이 좋음
- Narrowing으로 판정해주는 문법들 → 현재 변수의 타입이 뭔지 특정지을 수 있기만 하면 다 인정해줌
  - typeof 변수
  - 속성명 in 오브젝트자료
  - 인스턴스 instanceof 부모
- 아니면 assertion 문법 (타입 덮어쓰기)
  ```tsx
  function 내함수(x: number | string) {
    let array: number[] = [];
    array[0] = x as number;
  }

  내함수(123);
  ```
- 빠따 안 맞기 위한 assertion 문법의 용도
  1. Narrowing 할 때 씀 → 애매한 union 타입을 확정 지을 때
  2. 무슨 타입이 들어올지 100% 확실할 때 써야 함 → 버그 추적 불가능
  3. 가급적 if로 narrowing 처리하고 정말 잘 모르겠을 때 쓰는 게 좋음

## 타입도 변수에 담아쓰세요 type 키워드 써서 & readonly

- type alias 만드는 법
  ```tsx
  type Animal = string | number | undefined;

  let 동물: Animal = false;
  ```
- type 변수 작명 관습 → 일반 변수랑 차이를 두기 위해 PascalCase
- const 변수는 등호로 재할당만 막는 역할임 → const로 담은 object 수정은 자유롭게 가능
  - 그래서 readonly 키워드를 통해서 수정을 막을 수 있음
    ```tsx
    type Girlfriend = {
      readonly name: string;
    };

    const 여친: Girlfriend = {
      name: "엠버",
    };

    여친.name = "유라"; // error
    ```
- type 합치는 것도 가능
  ```tsx
  type Name = string;
  type Age = number;
  type Person = Name | Age;
  ```
- & 연산자로 object 타입 합치기(extend 하기)
  ```tsx
  type PositionX = { x: number };
  type PositionY = { y: number };

  type NewType = PositionX & PositionY;

  let position: NewType = { x: 10, y: 20 };
  ```
- type 키워드는 재정의가 불가능

**과제 체크 필요**
