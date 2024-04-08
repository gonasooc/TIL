[Jest 강좌](https://www.youtube.com/playlist?list=PLZKTXPmaJk8L1xCg_1cRjL5huINlP2JKt)

# Jest 강좌 #1 소개, 설치 및 간단한 테스트 작성

- Jest는 zero config 지향 → 별도의 설정 없이 빠르게 테스트 코드 작성 가능
- npm test 명령어를 실행 → 프로그램 내 모든 test 파일을 찾아서 테스트 실행

```jsx
// fn.js

const fn = {
  add: (num1, num2) => num1 + num2,
};

module.exports = fn;
```

```jsx
// fn.test.js

const fn = require("./fn");

test("1은 1이야.", () => {
  expect(1).toBe(1);
});

test("2 더하기 3은 5야.", () => {
  expect(fn.add(2, 3)).toBe(5);
});

test("3 더하기 3은 5야.", () => {
  expect(fn.add(3, 3)).toBe(5);
});
```

```
 FAIL  ./fn.test.js
  ✓ 1은 1이야. (1 ms)
  ✓ 2 더하기 3은 5야. (1 ms)
  ✕ 3 더하기 3은 5야. (1 ms)

  ● 3 더하기 3은 5야.

    expect(received).toBe(expected) // Object.is equality

    Expected: 5
    Received: 6

      10 |
      11 | test("3 더하기 3은 5야.", () => {
    > 12 |   expect(fn.add(3, 3)).toBe(5);
         |                        ^
      13 | });
      14 |

      at Object.toBe (fn.test.js:12:24)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 2 passed, 3 total
Snapshots:   0 total
Time:        0.182 s
Ran all test suites.
```

- toBe 부분에서 사용하는 함수를 Matcher, toBe는 숫자나 문자 등 기본 타입을 비교할 때 사용

# Jest 강좌 #2 유용한 Matchers

```jsx
const fn = {
  add: (num1, num2) => num1 + num2,
  makeUser: (name, age) => ({ name, age }),
};

module.exports = fn;
```

```jsx
const fn = require("./fn");

test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
  expect(fn.makeUser("Mike", 30)).toBe({
    name: "Mike",
    age: 30,
  });
});
```

```
 FAIL  ./fn.test.js
  ✕ 이름과 나이를 전달 받아서 객체를 반환해줘 (3 ms)

  ● 이름과 나이를 전달 받아서 객체를 반환해줘

    expect(received).toBe(expected) // Object.is equality

    If it should pass with deep equality, replace "toBe" with "toStrictEqual"

    Expected: {"age": 30, "name": "Mike"}
    Received: serializes to the same string

      2 |
      3 | test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
    > 4 |   expect(fn.makeUser("Mike", 30)).toBe({
        |                                   ^
      5 |     name: "Mike",
      6 |     age: 30,
      7 |   });

      at Object.toBe (fn.test.js:4:35)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
```

- 객체나 배열은 재귀적으로 돌면서 값을 확인해줘야 되기 때문에 toEqual을 사용
  ```jsx
  const fn = require("./fn");

  test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
    expect(fn.makeUser("Mike", 30)).toBe({
      name: "Mike",
      age: 30,
    });
  });

  test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
    expect(fn.makeUser("Mike", 30)).toEqual({
      name: "Mike",
      age: 30,
    });
  });
  ```
- If it should pass with deep equality, replace "toBe" with "toStrictEqual”
  ```
  const fn = {
    add: (num1, num2) => num1 + num2,
    makeUser: (name, age) => ({ name, age, gender: undefined }),
  };

  module.exports = fn;

  ```
  ```jsx
  const fn = require("./fn");

  test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
    expect(fn.makeUser("Mike", 30)).toEqual({
      name: "Mike",
      age: 30,
    });
  });

  test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
    expect(fn.makeUser("Mike", 30)).toStrictEqual({
      name: "Mike",
      age: 30,
    });
  });
  ```
  ```
   FAIL  ./fn.test.js
    ✓ 이름과 나이를 전달 받아서 객체를 반환해줘 (1 ms)
    ✕ 이름과 나이를 전달 받아서 객체를 반환해줘 (2 ms)

    ● 이름과 나이를 전달 받아서 객체를 반환해줘

      expect(received).toStrictEqual(expected) // deep equality

      - Expected  - 0
      + Received  + 1

        Object {
          "age": 30,
      +   "gender": undefined,
          "name": "Mike",
        }

         9 |
        10 | test("이름과 나이를 전달 받아서 객체를 반환해줘", () => {
      > 11 |   expect(fn.makeUser("Mike", 30)).toStrictEqual({
           |                                   ^
        12 |     name: "Mike",
        13 |     age: 30,
        14 |   });

        at Object.toStrictEqual (fn.test.js:11:35)

  Test Suites: 1 failed, 1 total
  Tests:       1 failed, 1 passed, 2 total
  Snapshots:   0 total
  Time:        0.172 s, estimated 1 s
  Ran all test suites.
  ```
  ```jsx
  const fn = require("./fn");

  // toBeNull
  // toBeUndefined
  // toBeDefined

  test("null은 null입니다.", () => {
    expect(null).toBeNull();
  });

  // toBeTruthy
  // toBeFalsy

  test("0은 false 입니다.", () => {
    expect(fn.add(1, -1)).toBeFalsy();
  });

  // toBeGreaterThan 크다
  // toBeGreaterThanOrEqual 크거나 같다
  // toBeLessThan 작다
  // toBeLessThanOrEqual 작거나 같다
  // 동일한 값을 비교할 땐 toBe나 toEqual

  test("ID는 10자 이하여야 합니다.", () => {
    const id = "THE_BLACK_ORDER";
    expect(id.length).toBeLessThanOrEqual(10);
  });

  // toBeCloseTo 2진법으로 무한 소수가 나오는 경우 비교

  test("0.1 더하기 0.2 는 0.3 입니다.", () => {
    expect(fn.add(0.1, 0.2)).toBeCloseTo(0.3);
  });

  // toMatch

  test("Hello World 에 a라는 글자가 있나?", () => {
    expect("Hello World").toMatch(/a/);
  });

  // toContain
  test("유저 리스트에 Mike가 있나?", () => {
    const user = "Mike";
    const userList = ["Tom", "Jane", "Kai", "Mike"];
    expect(userList).toContain(user);
  });
  ```
  # Jest 강좌 #3 비동기 코드 테스트
  ```jsx
  const fn = {
    add: (num1, num2) => num1 + num2,
    getName: (callback) => {
      const name = "Mike";
      setTimeout(() => {
        callback(name);
      }, 3000);
    },
  };

  module.exports = fn;
  ```
  ```jsx
  const fn = require("./fn");

  test("3초 후에 받아온 이름은 Mike", (done) => {
    function callback(name) {
      expect(name).toBe("Mike");
      done();
    }
    fn.getName(callback);
  });
  ```
  - callback의 경우 done을 통해서 종료 시점을 정해줌
  ```jsx
  const fn = {
    add: (num1, num2) => num1 + num2,
    getName: (callback) => {
      const name = "Mike";
      setTimeout(() => {
        callback(name);
      }, 3000);
    },
    getAge: () => {
      const age = 30;
      return new Promise((res, rej) => {
        setTimeout(() => {
          res(age);
        }, 3000);
      });
    },
  };

  module.exports = fn;
  ```
  ```jsx
  const fn = require("./fn");

  test("3초 후에 받아온 나이는 30", () => {
    return fn.getAge().then((age) => {
      expect(age).toBe(30);
    });
  });
  ```
  - Promise의 경우 return이 필요
  ```jsx
  const fn = require("./fn");

  test("3초 후에 받아온 나이는 30", () => {
    // return fn.getAge().then((age) => {
    //   expect(age).toBe(30);
    // });
    return expect(fn.getAge()).resolves.toBe(30);
  });
  ```
  - resolves 사용
  ```jsx
  const fn = require("./fn");

  test("3초 후에 에러가 납니다.", async () => {
    // const age = await fn.getAge();
    // expect(age).toBe(30);
    await expect(fn.getAge()).resolves.toBe(30);
  });
  ```
  - async, await와 resolves 사용
