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
  # Jest 강좌 #4 테스트 전후 작업
  ```jsx
  const fn = require("./fn");

  let num = 0;

  // 테스트 전에 작업
  // 전체 테스트 전에는 beforeAll
  beforeEach(() => {
    num = 0;
  });

  // 테스트 후에 작업
  // 전체 테스트 후에는 afterAll
  afterEach(() => {
    num = 0;
  });

  test("0 더하기 1은 1이야.", () => {
    num = fn.add(num, 1);
    expect(num).toBe(1);
  });

  test("0 더하기 2은 2이야.", () => {
    num = fn.add(num, 2);
    expect(num).toBe(2);
  });

  test("0 더하기 3은 3이야.", () => {
    num = fn.add(num, 3);
    expect(num).toBe(3);
  });

  test("0 더하기 4은 4이야.", () => {
    num = fn.add(num, 4);
    expect(num).toBe(4);
  });
  ```
  # Jest 강좌 #5 목 함수(Mock Functions)
  - mock function - 테스트 하기 위해 흉내만 내는 함수
  - 사용 목적: 작성해야 될 코드가 상당히 많아지는 경우, 혹은 네트워크 같은 외부 요인에 결과가 달라질 수 있는 경우, 동일한 코드가 동일한 결과를 내기 위해 목 함수를 사용
    ```jsx
    // mock function

    const mockFn = jest.fn();

    mockFn();
    mockFn(1);

    test("함수는 2번 호출됩니다.", () => {
      expect(mockFn.mock.calls.length).toBe(2);
    });

    test("두 번째로 호출된 함수에 전달된 첫 번째 인수는 1입니다.", () => {
      expect(mockFn.mock.calls[1][0]).toBe(1);
    });
    ```
  - 별도의 함수 대신 목 함수를 이용
    ```jsx
    // mock function

    const mockFn = jest.fn();

    function forEachAdd1(arr) {
      arr.forEach((num) => {
        mockFn(num + 1);
      });
    }

    forEachAdd1([10, 20, 30]);

    test("함수 호출은 세 번 됩니다.", () => {
      expect(mockFn.mock.calls.length).toBe(3);
    });

    test("전달된 값은 11, 21, 31 입니다.", () => {
      expect(mockFn.mock.calls[0][0]).toBe(11);
      expect(mockFn.mock.calls[1][0]).toBe(21);
      expect(mockFn.mock.calls[2][0]).toBe(31);
    });
    ```
  - results - return 값
    ```jsx
    // mock function

    const mockFn = jest.fn((num) => num + 1);

    mockFn(10);
    mockFn(20);
    mockFn(30);

    test("10에서 1 증가", () => {
      expect(mockFn.mock.results[0].value).toBe(11);
    });

    test("20에서 1 증가", () => {
      expect(mockFn.mock.results[1].value).toBe(21);
    });

    test("30에서 1 증가", () => {
      expect(mockFn.mock.results[2].value).toBe(31);
    });
    ```
  - mockReturnValueOnce - 별도의 return 값을 지정
    ```jsx
    // mock function

    const mockFn = jest.fn();

    mockFn
      .mockReturnValueOnce(true)
      .mockReturnValueOnce(false)
      .mockReturnValueOnce(true)
      .mockReturnValueOnce(false)
      .mockReturnValue(true);

    const result = [1, 2, 3, 4, 5].filter((num) => mockFn(num));

    test("홀수는 1, 3, 5", () => {
      expect(result).toStrictEqual([1, 3, 5]);
    });
    ```
  - mockResolvedValue - then을 이용해서 비동기 테스트
    ```jsx
    // mock function

    const mockFn = jest.fn();

    mockFn.mockResolvedValue({ name: "Mike" });

    test("받아온 이름은 Mike", () => {
      mockFn().then((res) => {
        expect(res.name).toBe("Mike");
      });
    });
    ```
  - toBeCalled - 한번이라도 호출되면 true
  - toBeCalledTimes - 정확한 호출 횟수
  - toBeCalledWith - 인수로 어떤 값을 받았는지 체크
  - lastCalledWith - 마지막으로 실행된 함수의 인수만 체크
    ```jsx
    // mock function

    const mockFn = jest.fn();

    mockFn(10, 20);
    mockFn();
    mockFn(30, 40);

    test("한번 이상 호출?", () => {
      expect(mockFn).toBeCalled();
    });

    test("정확히 세 번 호출?", () => {
      expect(mockFn).toBeCalledTimes(3);
    });

    test("10이랑 20 전달 받은 함수가 있는가?", () => {
      expect(mockFn).toBeCalledWith(10, 20);
    });

    test("마지막 함수는 30이랑 40 받았음?", () => {
      expect(mockFn).lastCalledWith(30, 40);
    });
    ```
