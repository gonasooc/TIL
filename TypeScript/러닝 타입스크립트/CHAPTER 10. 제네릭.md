# CHAPTER 10. 제네릭

- 지금까지 배운 모든 구문은 완전히 알려진 타입과 함께 사용해야 했는데, 때로는 호출하는 방식, 호출 영역에서 다양한 타입으로 작동하게 의도적으로 작성할 수 있습니다. 다른 언어에서 제네릭(generic)은 데이터의 타입을 일반화한다(generalize)는 의미로 사용되는데, 타입스크립트에서도 비슷합니다.
- 자바스크립트에서 다음 identity 함수는 모든 가능한 타입으로 input을 받고, 동일한 input을 출력으로 반환합니다.
  ```tsx
  function identity(input) {
    return input;
  }

  identity("abc");
  identity(123);
  identity("quote: blahblah");
  ```
- input이 모든 입력을 허용한다면, input의 타입과 함수의 반환 타입 간의 관계를 말할 수 있는 방법이 필요합니다. 타입스크립트는 제네릭(generic)을 사용해 타입 간의 관계를 알아냅니다.
- 타입 매개변수는 전형적으로 T나 U 같은 단일 문자 이름 또는 Key와 Value 같은 파스칼 케이스 이름을 갖습니다. 이 장에서 다루는 모든 구조체에서는 <, >를 사용해 someFunction<T> 또는 someInterface<T> 처럼 제네릭을 선언합니다.

# 10.1 제네릭 함수

- 매개변수 괄호 바로 앞 홑화살괄호(<, >)로 묶인 타입 매개변수에 별칭을 배치해 함수를 제네릭으로 만듭니다. 그러면 해당 타입 매개변수를 함수 본문 내부의 매개변수 타입 에너테이션, 반환 타입 애너테이션, 타입 애너테이션에서 사용할 수 있습니다.
- 다음 예시는 매개변수 T를 선언하고, 이를 통해 타입스크립트는 함수의 반환 타입이 T임을 유추합니다. 그러면 identity가 호출될 때마다 T에 대한 다른 타입을 유추할 수 있습니다.
  ```tsx
  function identity<T>(input: T) {
    return input;
  }

  const numeric = identity("me"); // 타입: "me"
  const stringy = identity(123); // 타입 123
  ```
- 이런 식으로 제네릭을 사용하면 타입 안정성을 유지하고 any 타입을 피하면서 다른 입력과 함께 재사용할 수 있습니다.

## 10.1.1 명시적 제네릭 호출 타입

- 제네릭 함수를 호출할 때 타입스크립트는 함수가 호출하는 방식에 따라서 인수를 유추합니다. 하지만 때론 호출 정보가 충분하거나, 타입 인수를 알 수 없는 제네릭 구문이 다른 제네릭 구문에 제공된 경우에는 **명시적 제네릭 타입 인수**를 사용해서 호출할 수도 있습니다.
  ```tsx
  function logWrapper<Input>(callback: (input: Input) => void) {
    return (input: Input) => {
      console.log("Input", input);
      callback(input);
    };
  }

  logWrapper<string>((input: string) => {
    console.log(input.length);
  });

  logWrapper<string>((input: boolean) => {
    // Argument of type '(input: boolean) => void' is not assignable to parameter of type '(input: string) => void'.
    // Types of parameters 'input' and 'input' are incompatible.
    // Type 'string' is not assignable to type 'boolean'.(2345)
  });
  ```

## 10.1.2 다중 함수 타입 매개변수

- 임의의 수의 타입 매개변수를 쉼표로 구분해 함수를 정의합니다.
  ```tsx
  function makeTuple<Fisrt, Second>(first: Fisrt, second: Second) {
    return [first, second] as const;
  }

  let tuple = makeTuple(true, "abc");
  ```
- 일부만을 명시적 제네릭 타입 선언을 통해 호출할 순 없습니다.
  ```tsx
  function makePair<Key, Value>(key: Key, value: Value) {
    return { key, value };
  }

  // 별도의 타입 인수가 제공되지 않음
  makePair("abc", 123);

  // 두 개의 타입 인수가 제공됨
  makePair<string, number>("abc", 123);
  makePair<"abc", 123>("abc", 123);

  // Expected 2 type arguments, but got 1.
  makePair<string>("abc", 123);
  ```

# 10.2 제네릭 인터페이스

- 인터페이스도 제네릭으로 선언 가능하고, 함수와 유사한 제네릭 규칙을 따릅니다.
  ```tsx
  interface Box<T> {
    inside: T;
  }

  let stringyBox: Box<string> = {
    inside: "abc",
  };

  let numberBox: Box<number> = {
    inside: 123,
  };

  // Type 'boolean' is not assignable to type 'number'.
  let incorrectBox: Box<number> = {
    inside: false,
  };
  ```
- 타입스크립트에서 내장 Array 메서드는 제네릭 인터페이스로 정의됩니다. Array는 타입 매개변수 T를 사용해서 배열 안에 저장된 데이터의 타입을 나타냅니다.
  ```tsx
  interface Array<T> {
    // 마지막 요소 제거하고 반환
    // 배열이 비어 있는 경우 undefined 반환하고 배열은 수정되지 않음
    pop(): T | undefined;

    // 배열의 마지막에 새로운 요소를 추가하고 배열의 길이를 반환
    // @param items 배열에 추가된 새로운 요소
    push(...itemA: T[]): number;
  }
  ```

## 10.2.1 유추된 제네릭 인터페이스 타입

- 제네릭 함수와 마찬가지로 선언된 위치에 제공된 값의 타입에서 타입 인수를 유추합니다.
- 다음 getLast 함수는 타입 매개변수 Value를 선언한 다음 Value를 node 매개변수로 사용합니다. 그러면 타입스크립트는 안수로 전달된 값의 타입에 따라 Value를 유추합니다.
  ```tsx
  interface LinkedNode<Value> {
      next?: LinkedNode<Value>;
      value: Value;
  }

  function getLast<Value>(node: LinkedNode<Value>): Value {
      return node.next ? getLast(node.next) : node.value;
  }

  // 유추된 Value 타입 인수: Date
  let lastDate = getLast({
      value: new Date("09-13-1993"),
  })

  // 유추된 Value 타입 인수: string
  let lastFruit = getLast({
      next: {
          value: "banana";
      },
      value: "apple",
  });

  // 유추된 Value 타입 인수: number
  let lastMisMatch = getLast({
      next: {
          value: 123,
      },
      value: false,
      // Type 'boolean' is not assignable to type 'number'.
  })
  ```
- 인터페이스가 타입 매개변수를 선언하는 경우, 해당 인터페이스를 참조하는 모든 타입 애너테이션은 이어 상응하는 타입 인수를 제공해야 합니다.
  ```tsx
  interface CreateLike<T> {
    content: T;
  }

  // Generic type 'CreateLike<T>' requires 1 type argument(s).
  let missingGeneric: CreateLike = {
    inside: "??",
  };
  ```

# 10.3 제네릭 클래스

- 추후 체크 예정

# 10.4 제네릭 타입 별칭

- 타입 별칭에는 T를 받는 Nullish 타입과 같은 임의의 수의 타입 매개변수가 주어집니다.
  ```tsx
  type Nullish<T> = T | null | undefined;
  ```
- 제네릭 타입 별칭은 일반적으로 제네릭 함수의 타입을 설명하는 함수와 함께 사용됩니다.
  ```tsx
  type CreatesValue<Input, Output> = (input: Input) => Output;

  // 타입: (input: string) => number
  let creator: CreatesValue<string, number>;

  creator = (text) => text.length; // ok

  creator = (text) => text.toUpperCase();
  ```

# ~~10.5 제네릭 제한자~~

## ~~10.5.1 제네릭 기본값~~

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)
- https://youtu.be/pReXmUBjU3E?si=Hzpie0yO8ZSQDtaR
