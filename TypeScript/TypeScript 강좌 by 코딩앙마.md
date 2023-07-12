# \***\*TypeScript #1 타입스크립트를 쓰는 이유를 알아보자\*\***

- 잘못된 타입, 매개변수를 던져줘서 잘못된 답이 나더라도 유연한(?) 탓에 디버깅이 어려움
- JavaScript (동적언어) : 런타임에 타입 결정 / 오류 발견 → 사용자가 오류를 고스란히 볼 수 있음
- Java, TypeScript (정적언어) : 컴파일 타임에 타입 결정 / 오류 발견 → 처음에 개발이 오래 걸릴 수 있으나 초기에 잘 잡아두면 안정적으로 개발 가능
- JS
  ```jsx
  function add(num1, num2) {
    console.log(num1, num2);
  }

  add();
  add(1);
  add(1, 2);
  add(3, 4, 5);
  add("hello", "world");
  ```
- TS

  ```tsx
  function add(num1: number, num2: number) {
    console.log(num1, num2);
  }

  // add();
  // add(1);
  add(1, 2);
  // add(3, 4, 5);
  // add(1, 'world');
  ```

- 함수를 만들 때 의도했던 방식 외에 다른 방법은 모두 에러라고 표시(주석한 부분들이 의도와 다른)

- JS
  ```jsx
  function showItems(arr) {
    arr.forEach((item) => {
      console.log(item);
    });
  }

  showItems([1, 2, 3]);
  showItems(1, 2, 3);
  ```
- TS
  ```tsx
  function showItems(arr: number[]) {
    arr.forEach((item) => {
      console.log(item);
    });
  }

  showItems([1, 2, 3]);
  // showItems(1, 2, 3)
  ```

# \***\*TypeScript #2 기본 타입\*\***

- 타입 추론을 하기도 함, 별도의 타입 선언 없어도 값을 인지해서 타입 추론

## 기본 타입

```tsx
let age: number = 30;
let isAdult: boolean = true;
let a: number[] = [1, 2, 3];
let a2: Array<number> = [1, 2, 3];

let week: string[] = ["mon", "tue", "wed"];
let week2: Array<string> = ["mon", "tue", "wed"];
```

## 튜플 (Tuple)

```tsx
let b: [string, number];

b = ["z", 1];
// b = [1, z]; // wrong

b[0].toLowerCase();
// b[1].toLowerCase(); // wrong
```

- TS가 깔려 있는 상황이면 b[1]에 . 찍었을 때 가능한 메소들이 타입을 고려해서 추려서 나옴(즉, toLowerCase()는 나오지 않음)

## void, never

### void는 함수에서 값을 반환하지 않을 때 주로 사용

```tsx
function sayHello(): void {
  console.log("hello");
}
```

### never는 항상 에러를 반환하거나 영원히 끝나지 않는 함수의 타입으로 사용할 수 있음

```tsx
function showError(): never {
  throw new Error();
}

function infLoop(): never {
  while (true) {
    // do shmething ..
  }
}
```

## enum

- TS
  ```tsx
  enum Os {
    Window = 3,
    Ios = 10,
    Android,
  }
  ```
- JS
  ```
  "use strict";
  var Os;
  (function (Os) {
      Os[Os["Window"] = 3] = "Window";
      Os[Os["Ios"] = 10] = "Ios";
      Os[Os["Android"] = 11] = "Android";
  })(Os || (Os = {}));
  ```
- TS
  ```tsx
  enum Os {
    Window = "win",
    Ios = "ios",
    Android = "and",
  }

  let myOs: Os;

  myOs = Os.Window;
  ```

## null, undefined

- TS
  ```tsx
  let a: null = null;
  let b: undefined = undefined;
  ```

# \***\*TypeScript #3 인터페이스(interface)\*\***

- 객체의 name의 타입이 엇어서 에러
  ```tsx
  let user: object;

  user = {
    name: "xx",
    age: 30,
  };

  console.log(user.name);
  ```
- property를 정의해서 객체로 표현하고자 할 때는 인터페이스를 사용
  ```tsx
  interface User {
    name: string;
    age: number;
    gender?: string; // optional property : ?를 사용함으로서 있어도, 없어도 됨
  }

  let user: User = {
    name: "xx",
    age: 30,
  };

  user.age = 10;
  user.gender = "main";
  ```
- 최초에 값을 할당하고 재할당을 못하게 하기 위해서는 readonly
  ```tsx
  interface User {
    name: string;
    age: number;
    gender?: string; // optional property : ?를 사용함으로서 있어도, 없어도 됨
    readonly birthYear: number;
  }

  let user: User = {
    name: "xx",
    age: 30,
    birthYear: 2000,
  };

  user.age = 10;
  user.gender = "main";
  user.birthYear = 2000; // error
  ```
- optional property : ?를 사용함으로서 있어도, 없어도 됨
  ```tsx
  interface User {
    name: string;
    age: number;
    gender?: string; // optional property : ?를 사용함으로서 있어도, 없어도 됨
    readonly birthYear: number;
    1?: string;
    2?: string;
    3?: string;
    4?: string;
  }

  let user: User = {
    name: "xx",
    age: 30,
    birthYear: 2000,
    1: "A",
  };

  user.age = 10;
  user.gender = "main";
  user.birthYear = 2000;
  ```
- interface로 함수도 정의 가능
  ```tsx
  interface Add {
    (num1: number, num2: number): number;
  }

  const add: Add = function (x, y) {
    return x + y;
  };

  add(10, 20);

  interface IsAdult {
    (age: number): boolean;
  }

  const a: IsAdult = (age) => {
    return age > 19;
  };

  a(33); // true
  ```
- implements
  ```tsx
  interface Car {
    color: string;
    wheels: number;
    start(): void;
  }

  class Bmw implements Car {
    color;
    wheels = 4;

    constructor(c: string) {
      this.color = c;
    }

    start() {
      console.log("go...");
    }
  }

  const b = new Bmw("green");
  console.log(b);
  b.start();
  ```

# TypeScript #4 함수

- return 갑이 있으면
  ```tsx
  function add(num1: number, num2: number): number {
    return num1 + num2;
  }
  ```
- 반환하는 값이 없으면
  ```tsx
  function add(num1: number, num2: number): void {
    // return num1 + num2;
    console.log(num1 + num2);
  }
  ```
- 반환값이 boolean
  ```tsx
  function isAdult(age: number): boolean {
    return age > 19;
  }
  ```
- 예외처리
  ```tsx
  function hello(name: string) {
    return `Hello, ${name || "world`"}`;
  }

  // name이 없을 걸 대비해서 코드를 짰지만,
  // TS에서는 보다 명시적으로 적어줘야 함
  const result = hello(); // error
  ```
  ```tsx
  function hello(name?: string) {
    // ?를 붙임으로서 에러가 사라짐
    return `Hello, ${name || "world`"}`;
  }

  // name이 없을 걸 대비해서 코드를 짰지만,
  // TS에서는 보다 명시적으로 적어줘야 함
  const result = hello();
  ```
- 나머지 매개변수
  ```tsx
  function add(...nums: number[]) {
    return nums.reduce((result, num) => result + num, 0);
  }

  add(1, 2, 3); // 6
  add(1, 2, 3, 4, 5, 6, 7, 8, 9, 10); // 55
  ```
- this - 매개변수 자리에 this를 정의하고 해당 값을 넣어주면 됨
  ```tsx
  interface User {
      name: string;
  }

  const Sam: User = {name: 'Sam'};

  function showName(**this:User**) { // this line
      console.log(this.name);
  }

  const a = showName.bind(Sam);
  a();
  ```
  ```tsx
  interface User {
    name: string;
  }

  const Sam: User = { name: "Sam" };

  function showName(this: User, age: number, gender: "m" | "f") {
    console.log(this.name, age, gender);
  }

  const a = showName.bind(Sam);
  a(30, "m");
  ```

# TypeScript #5 리터럴, 유니온/교차 타입

- 유니온 타입을 통해 명시적으로 숫자를 재할당해줄 수 있음
  ```tsx
  const userName1 = "Bob";
  let userName2: string | number = "Tom";
  userName2 = 3;
  ```
- type, interface
  ```tsx
  const userName1 = "Bob";
  let userName2: string | number = "Tom";
  userName2 = 3;

  type Job = "police" | "developer" | "teacher";

  interface User {
    name: string;
    job: Job;
  }

  const user: User = {
    name: "Bob",
    job: "student", // error
  };
  ```
  ```tsx
  const userName1 = "Bob";
  let userName2: string | number = "Tom";
  userName2 = 3;

  type Job = "police" | "developer" | "teacher";

  interface User {
    name: string;
    job: Job;
  }

  const user: User = {
    name: "Bob",
    job: "developer", // error
  };

  interface HighSchoolStudent {
    name: string;
    grade: 1 | 2 | 3;
  }
  ```
- 식별 가능한 Union type

  ```tsx
  interface Car {
    name: "car";
    color: string;
    start(): void;
  }

  interface Mobile {
    name: "mobile";
    color: string;
    call(): void;
  }

  function getGift(gift: Car | Mobile) {
    console.log(gift.color);
    if (gift.name === "car") {
      gift.start();
    } else {
      gift.call();
    }
  }
  ```

- Intersection Types(교치타입) → 교차타입은 여러 개의 타입을 하나로 합쳐주는 역할을 함 → 하나라도 없으면 error
  ```tsx
  interface Car {
    name: string;
    start(): void;
  }

  interface Toy {
    name: string;
    color: string;
    price: number;
  }

  const toyCar: Toy & Car = {
    name: "타요",
    start() {},
    color: "blue",
    price: 1000,
  };
  ```

# TypeScript #6 클래스 Class

- member 변수 미리 선언 필요함(물론 매개변수 타입도 선언 필요)
  ```tsx
  class Car {
    color: string;
    constructor(color: string) {
      this.color = color;
    }
    start() {
      console.log("start");
    }
  }

  const bmw = new Car("red");
  ```
  ```tsx
  class Car {
    // color: string;
    constructor(public color: string) {
      this.color = color;
    }
    start() {
      console.log("start");
    }
  }

  const bmw = new Car("red");
  ```

# TypeScript #7 Generic

- 유니온 타입으로 모든 걸 처리하는 건 다소 비효율적일 수 있음

  ```tsx
  function getSize(arr: number[] | string[] | boolean[] | object[]): number {
    return arr.length;
  }

  const arr1 = [1, 2, 3];
  getSize(arr1); // 3

  const arr2 = ["a", "b", "c"];
  getSize(arr2); // 3

  const arr3 = [false, true, true];
  getSize(arr3); // 3

  const arr4 = [{}, {}, { name: "TIm" }];
  getSize(arr4); // 3
  ```

- 이럴 때 사용하는 게 generic → getSize 뒤에 타입 파라미터를 통해 타입을 정의하고 그 타입을 사용할 수 있게 해줌
- 다만 generic을 사용했다고 해서 꼭 명시해줄 필요는 없는 게 타입 추론을 통해 이미 들어오는 매개변수를 통해 타입이 결정됨
  ```tsx
  function getSize<T>(arr: T[]): number {
    return arr.length;
  }

  const arr1 = [1, 2, 3];
  getSize<number>(arr1); // 3

  const arr2 = ["a", "b", "c"];
  getSize<string>(arr2);

  const arr3 = [false, true, true];
  getSize(arr3);

  const arr4 = [{}, {}, { name: "TIm" }];
  getSize(arr4);
  ```
  ```tsx
  interface Mobile<T> {
    name: string;
    price: number;
    option: T;
  }

  const m1: Mobile<object> = {
    name: "s21",
    price: 1000,
    option: {
      color: "red",
      coupon: false,
    },
  };

  const m2: Mobile<string> = {
    name: "s20",
    price: 900,
    option: "good",
  };
  ```
- generic 예시
  ```tsx
  interface User {
    name: string;
    age: number;
  }

  interface Car {
    name: string;
    color: string;
  }

  interface Book {
    price: number;
  }

  const user: User = { name: "a", age: 10 };
  const car: Car = { name: "bmw", color: "red" };
  const book: Book = { price: 3000 };

  function showName<T extends { name: string }>(data: T): string {
    return data.name;
  }

  showName(user);
  showName(car);
  // showName(book);
  ```

# TypeScript #8 유틸리티 타입 Utility Types

```tsx
interface User {
  id: number;
  name: string;
  age: number;
  gender: "m" | "f";
}

type UserKey = keyof User; // 'id' | 'name' | 'age' | 'gender'

const uk: UserKey = ""; // error
```

- Partial, Required, Readonly, Record, Pick, Omit, Exclude, NonNullable
