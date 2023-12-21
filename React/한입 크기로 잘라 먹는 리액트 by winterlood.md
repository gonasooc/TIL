# 섹션 1. JavaScript 기본

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

## 객체

- new 연산자와 생성자 함수보다는 객체 리터럴을 통해 주로 생성
- `const`로 선언된 `person` 자체를 수정하는 게 아니라 `person` 객체 `property`의 `value`를 수정하는 건 오류가 나지 않음
  ```jsx
  const person = {
    name: "최관수",
    age: 25,
  };

  person.location = "한국";
  console.log(person["location"]);
  ```
- `delete`를 통해 객체를 삭제할 수 있지만 실제 객체와 `property`와의 연결을 끊을 뿐 메모리 할당은 그대로 되고 있음, 메모리까지 지우고자 하면 `null`를 할당
  ```jsx
  const person = {
    name: "최관수",
    age: 25,
  };

  delete person.age;
  person.name = null;

  console.log(person);
  ```
- 객체 안에 함수인 `property`는 메서드, 함수가 있는 `property`는 멤버라고 부름
- this를 사용해서 객체 내에 다른 property를 바라보게 할 수 있음
  ```jsx
  const person = {
    name: "최관수",
    age: 25,
    say: function () {
      console.log(`안녕 난 ${this.name}`);
    },
  };

  person.say();
  ```
- property in object로 boolean 반환 가능
  ```jsx
  const person = {
    name: "최관수",
    age: 25,
    say: function () {
      console.log(`안녕 난 ${this.name}`);
    },
  };

  person.say();
  console.log("age" in person); // true
  ```

## 배열

- new 연산자와 생성자 함수보다는 배열 리터럴을 통해 주로 생성

## 반복문

- 객체를 순회하는 방법
  - Object.keys(object) → 객체의 key들을 배열로 반환
  - Object.values(object) → 객체의 value들를 배열로 반환
  ```jsx
  let person = {
    name: "최관수",
    age: 25,
    tall: 180,
  };

  const personKeys = Object.keys(person);

  for (let i = 0; i < personKeys.length; i++) {
    console.log(personKeys[i]); // key 선회
    console.log(person[personKeys[i]]); // value 선회
  }
  ```

### 배열 내장함수

- forEach, map, includes, indexOf, findIndex, find, filter, slice, concat, sort, join

# 섹션 2. JavaScript 응용

## Truthy & Falsy

- true가 아니어도 참으로 평가되는 요소, Truthy - ex) [], {}, 42, Infinity..
- false가 아니어도 거짓으로 평가되는 요소, Falsy - null, undefined, 0, -0, NaN, “”
  ```jsx
  const getName = (person) => {
    if (!person) {
      return "객체가 아닙니다.";
    }
  };

  let person = null;
  const name = getName(person);
  console.log(name);
  ```

## 단락 회로 평가

- 왼쪽에서 오른쪽으로 연산하게 되는 논리 연산자의 연산 순서를 이용하는 문법
  ```jsx
  console.log(true && true);
  console.log(true || false);
  console.log(!true);
  ```
  ```jsx
  const getName = (person) => {
    const name = person && person.name;
    return name || "객체가 아닙니다";
  };

  let person = { name: "최관수" };
  const name = getName(person);
  console.log(name);
  ```

## 비 구조화 할당(구조분해 할당)

- 예시
  ```jsx
  let arr = ["one", "two", "three"];

  // let one = arr[0];
  // let two = arr[1];
  // let three = arr[2];

  let [one, two, three] = arr;
  console.log(one, two, three);
  ```
- swap에도 활용할 수 있음
  ```jsx
  let a = 10;
  let b = 20;

  [a, b] = [b, a];
  console.log(a, b);
  ```
- 객체의 비 구조화 할당
  ```jsx
  let object = { one: "one", two: "two", three: "three" };
  let { one, two, three } = object;
  console.log(one, two, three);
  ```
- 이름을 바꿔서 할당 받을 수 있음
  ```
  let object = { one: "one", two: "two", three: "three" };
  let { one: myName, two, three } = object;
  console.log(myName, two, three);
  ```

## spread 연산자

- 객체의 spread 연산자
  ```jsx
  const cookie = {
    base: "cookie",
    madeIn: "korea",
  };

  const chocochipCookie = {
    ...cookie,
    toping: "chocechip",
  };

  const blueberryCookie = {
    ...cookie,
    toping: "blueberry",
  };

  const strawberryCookie = {
    ...cookie,
    toping: "strawberry",
  };

  console.log(chocochipCookie);
  ```
- 배열의 spread 연산자
  ```jsx
  const noTopingCookies = ["촉촉한쿠키", "안촉촉한쿠키"];
  const topingCookies = [
    "바나나쿠키",
    "블루베리쿠키",
    "딸기쿠키",
    "초코칩쿠키",
  ];

  const allCookies = [...noTopingCookies, "함정쿠키", ...topingCookies];
  console.log(allCookies);
  ```

## 동기 & 비동기

- 자바스크립트의 **싱글 스레드** 작업 수행 방식(**블로킹 방식**)
  - 자바스크립트는 코드가 작성된 순서대로 작업을 처리함
  - 이전 작업이 진행 중일 때는 다음 작업을 수행하지 않고 기다림
  - 먼저 작성된 코드를 먼저 다 실행하고 나서 뒤에 작성된 코드를 실행한다.
- 동기 처리 방식은 하나의 작업이 너무 오래 걸리게 되면 모든 작업이 오래 걸리게 되기 때문에 전반적인 흐름이 느려짐
- 자바스크립트는 **싱글 스레드**이기 때문에 멀티 스레드가 될 순 없고, **논 블로킹 방식**으로 **비동기 실행 방식**을 통해 여러 개의 작업을 동시에 실행 → 콜백 함수를 통해 비동기 처리의 결과값을 이용해서 호출하게 됨
- 비동기 방식 예시
  ```jsx
  function taskA() {
    setTimeout(() => {
      console.log("A TASK END");
    }, 2000);
  }

  taskA();
  console.log("코드 끝");
  ```
  ```jsx
  function taskA(a, b, cb) {
    setTimeout(() => {
      const res = a + b;
      cb(res);
    }, 3000);
  }

  function taskB(a, cb) {
    setTimeout(() => {
      const res = a * 2;
      cb(res);
    }, 1000);
  }

  function taskC(a, cb) {
    setTimeout(() => {
      const res = a * -1;
      cb(res);
    }, 2000);
  }

  taskA(3, 4, (res) => {
    console.log("A TASK", res);
  });

  taskB(7, (res) => {
    console.log("B TASK", res);
  });

  taskC(7, (res) => {
    console.log("C TASK", res);
  });

  console.log("코드 끝");
  ```

## Promise

- 비동기 작업이 가질 수 있는 3가지 상태
  - Pending(대기) / Fulfilled(성공) / Rejected(실패)
- 비동기 예시
  ```jsx
  function isPositive(number, resolve, reject) {
    setTimeout(() => {
      if (typeof number === "number") {
        // 성공 resolve
        resolve(number > 0 ? "양수" : "음수");
      } else {
        // 실패 reject
        reject("주어진 값이 숫자형 값이 아닙니다.");
      }
    }, 2000);
  }

  isPositive(
    [],
    (res) => {
      console.log("성공적 수행: ", res);
    },
    (err) => {
      console.log("실패하였음: ", err);
    }
  );
  ```
- Promise
  ```jsx
  function isPositive(number, resolve, reject) {
    setTimeout(() => {
      if (typeof number === "number") {
        // 성공 resolve
        resolve(number > 0 ? "양수" : "음수");
      } else {
        // 실패 reject
        reject("주어진 값이 숫자형 값이 아닙니다.");
      }
    }, 2000);
  }

  function isPositiveP(number) {
    const executor = (resolve, reject) => {
      setTimeout(() => {
        if (typeof number === "number") {
          // 성공 resolve
          console.log(number);
          resolve(number > 0 ? "양수" : "음수");
        } else {
          // 실패 reject
          reject("주어진 값이 숫자형 값이 아닙니다.");
        }
      }, 2000);
    };

    const asyncTask = new Promise(executor);
    return asyncTask;
  }

  const res = isPositiveP([]);

  res
    .then((res) => {
      console.log("작업 성공: ", res);
    })
    .catch((err) => {
      console.log("작업 실패: ", err);
    });
  ```

## async & await

- async 붙이면 Promise를 반환하고, resolve의 값으로 함수 내부의 값을 반환
  ```jsx
  // async

  function hello() {
    return "hello";
  }

  async function helloAsync() {
    return "hello Async";
  }

  console.log(hello());
  // console.log(helloAsync());

  const res = helloAsync();

  res
    .then((res) => {
      console.log(res);
    })
    .catch((err) => {
      console.log(err);
    });
  ```
- await - 비동기를 동기적으로 수행(기다렸다가 수행)
  ```jsx
  function delay(ms) {
    return new Promise((resolve) => {
      setTimeout(resolve, ms);
    });
  }

  async function helloAsync() {
    await delay(3000);
    return "hello async";
  }

  async function main() {
    const result = await helloAsync();
    console.log(result);
  }

  main();
  ```

## API & fetch

- 기본 예시
  ```jsx
  async function getData() {
    let rawResponse = await fetch("https://jsonplaceholder.typicode.com/posts");
    let jsonResponse = await rawResponse.json();
    console.log(jsonResponse);
  }

  getData();
  ```

# 섹션 3. Node.js 기초

## Node.js란?

- 자바스크립트 코드는 브라우저 내장 자바스크립트 엔진을 이용하여 실행
- V8 엔진을 브라우저 외부에서도 실행할 수 있도록 하는 게 node.js

## Node.js Hello World & Common JS

- Node.js에서의 모듈 호출(Common.js 모듈 시스템)
  ```jsx
  // calc.js

  const add = (a, b) => {
    return a + b;
  };

  const sub = (a, b) => {
    return a - b;
  };

  module.exports = {
    moduleName: "calc module",
    add: add,
    sub: sub,
  };
  ```
  ```jsx
  // index.js

  const calc = require("./calc");

  console.log(calc.add(1, 2));
  console.log(calc.add(4, 5));
  console.log(calc.add(10, 2));
  ```

## Node.js 패키지 생성 및 외부 패키지 사용하기

- Package - 모듈의 모음, 패키지
- package 만들 폴더를 생성하고, 해당 폴더 root에서 npm init을 통해 패키지 초기 설정
  - package name - 원하는 패키지 이름
  - version - 별도의 version 기입 없으면 엔터로 default 설정
  - description - 필수 입력값은 아님, 엔터로 default 설정
  - entry point, test command, git repository, keywords, author, license..
- package.json이라는 파일 하나가 생성되면서 초기 설정 완료

```json
{
  "name": "package-example1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js", // 최초 진입 파일 경로
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1" // 해당 명령어로 스크립트 수행
		"start": "node index.js" // 이런 식으로 추가 가능
},
  "author": "gonasooc",
  "license": "ISC"
}
```

- 별도의 패키지를 설치하는 경우 dependencies 추가

```json
{
  "name": "package-example1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js"
  },
  "author": "gonasooc",
  "license": "ISC",
  "dependencies": {
    "randomcolor": "^0.6.2"
  }
}
```

- node_modules - 외부 패키지의 보관소
- package-lock.json - package.json은 Version Range Syntax를 통해 패키지 버전의 범위를 표기했다면, package-lock.json에서는 정확히 몇 버전인지 확인 가능
- 사용 예시
  ```jsx
  var randomColor = require("randomcolor");

  let color1 = randomColor();
  let color2 = randomColor();
  let color3 = randomColor();

  console.log(color1, color2, color3);
  ```

# 섹션 4. React.js 기초

## Create React App
