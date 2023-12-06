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

# JavaScript 응용

## Truthy & Falsy

- true가 아니어도 참으로 평가되는 요소, Truthy