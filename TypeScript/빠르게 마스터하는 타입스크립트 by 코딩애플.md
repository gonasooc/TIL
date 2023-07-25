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

## Literal Types로 만드는 const 변수 유사품

- 더 엄격한 타입 지정 가능 → ex) “kim or park 문자만 들어올 수 있습니다~” Literal Types
  ```tsx
  let 이름: 123;
  이름 = 456; // error
  ```
- LIteral Types
  - 변수에 뭐가 들어올지 더 엄격하게 관리 가능
  - 버그 방지 & 자동 완성
- 함수의 parameter 뿐만 아니라 return 값도 제한할 수 있어서 버그를 미연에 방지할 수 있음
  ```tsx
  const game = (hand: "가위" | "바위" | "보"): ("가위" | "바위" | "보")[] => {
    return [hand];
  };
  ```
- Literal type은 const 변수와 유사하게 사용 가능 → const 변수 업그레이드 버전 같은 느낌

## 함수와 methods에 type alias 지정하는 법

- 함수 type alias 부착하려면 함수표현식 써야
  1. 함수 타입은 () ⇒ {} 모양으로
  2. 함수 type alias 부착하려면 함수표현식 써야

**과제..**

## 타입스크립트로 HTML 변경과 조작할 때 주의점

- 변수에 element를 담으면 조작할 때 error가 생김 → 해당 element가 제대로 담기지 않았을 때는 null이고 제대로 담기면 element라서 narrowing으로 명확하게 처리해줘야 함
  ```tsx
  let 제목 = document.querySelector("#title");
  제목.innerHTML = "반가워요"; // error
  ```
  ```
  let 제목 = document.querySelector("#title");
  if(제목 !== null) {
    제목.innerHTML = "반가워요";
  }
  ```
- HTML 조작시 narrowing 방법 5개
  - instanceof 연산자
    ```tsx
    let 제목 = document.querySelector("#title");
    if (제목 instanceof Element) {
      제목.innerHTML = "반가워요";
    }
    ```
  - as로 사기치기 → null이 들어와도 무조건 타입이 Element로 확정 시키기
    ```tsx
    let 제목 = document.querySelector("#title") as Element;
    제목.innerHTML = "반가워요";
    ```
  - 오브젝트에 붙이는 ?. (optional chaining)
    ```tsx
    let 제목 = document.querySelector("#title");
    if (제목?.innerHTML !== undefined) {
      제목.innerHTML = "반가워요";
    }
    ```
  - 귀찮은 strict 모드 자체를 끌 수도 있음
    ```json
    {
      "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "strictNullChecks": false
      }
    }
    ```
  - narrowing 예시
    ```tsx
    let 링크 = document.querySelector(".link");
    if (링크 instanceof HTMLAnchorElement) {
      링크.href = "http://kakao.com";
    }
    ```
    ```tsx
    let 버튼 = document.querySelector("#button");
    버튼?.addEventListener("click", function () {});
    ```

## (JS문법시간) class 키워드 알아보기

[객체지향 Class 문법 10분만에 이해시켜줌 (자바스크립트)](https://youtu.be/dHrI-_xq1Vo)

- **“저렇게 비슷한 object 많이 만들 일 있으면 class 만들어쓰세요” → class는 object 뽑는 기계라고 생각하면 좋음**
- class 문법이 없던 시절에는 function으로 구현
  ```jsx
  function 기계() {
    this.q = "consume"; // '새로 생성되는 object에 { q: 'consume' } 추가해주셈'
    this.w = "snowball";
  }
  ```
- this는 기계로부터 생성되는 object라고 생각하면 좋음(멋진 말로 instance)
  ```jsx
  function 기계(구멍) {
    this.q = 구멍;
    this.w = "snowball";
  }

  let nunu = new 기계("consume");
  let garen = new 기계("strike");

  class Hero {
    constructor(구멍) {
      this.q = 구멍;
      this.w = "snowball";
    }
  }

  let test = new Hero("hero");
  console.log(test);
  ```

## (JS 문법시간) prototype 문법 짚어보기

[이거보고 prototype 이해 못하면 강의접음](https://youtu.be/wUgmzvExL_E)

- prototype은 유전자 역할을 함
- object에서 자료 뽑을 때 일어나는 일
  1. 직접 자료를 가지고 있으면 그거 출력
  2. 없으면 부모 유전자까지 뒤짐
  3. 없으면 부모의부모유전자까지
  4. prototype chain..
- Array의 내장함수들은 prototype으로 들고 있음 ex) `Array.prototype.sort()`
- prototype에 함수 추가하는 것도 가능 ex) `Array.prototype.함수 = function() {}`

## class 만들 때 타입지정 가능

- TypeScript constructor() → 필드값에 **어쩌구**가 미리 있어야 this.**어쩌구** 가능
  ```tsx
  class Person {
    name: string;

    constructor(a: string) {
      this.name = a;
    }
  }

  let 사람1 = new Person("Kim");
  let 사람2 = new Person("Park");
  console.log(사람2);
  ```
- constructor의 parameter는 type 지정이 필요하지만 class 문법을 통해 생성되는 건 항상 object라 return 타입 지정할 필요 없음
  ```tsx
  class Person {
    name: string;

    constructor(a: string) {
      this.name = a;
    }

    함수(a: string): void {
      // prototype에 함수 추가
      console.log("안녕" + a);
    }
  }

  let 사람1 = new Person("Kim");
  let 사람2 = new Person("Park");
  console.log(사람2.함수);
  사람1.함수("HI");
  ```

## Object에 타입지정하려면 interface 이것도 있음

- object의 경우 type, interface 둘 다 사용 가능
- class 만드는 문법과 비슷해서 interface는 등호가 필요 없음
- interface의 장점: extends로 복사 가능
  ```tsx
  interface Student {
    name: string;
  }

  interface Teacher extends Student {
    age: number;
  }

  let 학생: Student = {
    name: "kim",
  };

  let 선생: Teacher = {
    name: "kim",
    age: 20,
  };
  ```
- 물론 type alias도 비슷하게 만들 수 있음 → & 기호를 사용(intersection)
  - interface의 extends와는 조금 차이가 있음
  - & 기호(intersection type)을 통해 두 타입을 전부 만족하는 타입이라는 거지 확장된 개념이 아님
  - interface 또한 & 기호로 붙일 수 있음
    ```tsx
    type Animal = {
      name: string;
    };

    type Cat = {
      age: number;
    } & Animal;
    ```
- **type vs interface (엄격함 vs 유연함)**
  - interface는 중복선언 가능 → 중복 선언을 하는 경우 두 개가 합쳐짐 → 일종의 자동 extends
  - type은 중복 선언 불가능
  - 외부 라이브러리 같은 경우 interface 많이 씀 → 사용자가 타입을 쉽게 추가할 수 있게끔
  - interface는 extends를 사용했을 때 동일한 속성이 있는 경우 error을 뱉어주나, type의 intersection을 사용했을 때는 error가 아니라 두 속성을 합쳐줌(?) → 때문에 interface가 조금 더 안전함
