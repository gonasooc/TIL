# 9.1 top 타입

- top 타입은 bottom 타입(ex) `never`)의 반대 개념으로, 시스템에서 가능한 모든 값을 나타내는 타입입니다. 모든 타입은 top 타입에 할당할 수 있습니다.

## 9.1.1 any 다시 보기

- `any`는 모든 타입을 받아들이기 때문에 타입 검사기를 빠르게 건너뛰려고 할 때 유용하지만, 해당 값에 대한 타입스크립트의 유용성이 줄어듭니다.
- 예컨대 아래의 코드에서 string 타입에서 사용하는 `name.toUpperCase()` 호출은 문제가 되지만, `name`이 `any`로 선언되었기 때문에 타입 오류를 보고하지 않습니다.
  ```tsx
  function greetComedian(name: any) {
    console.log(`Announcing ${name.toUpperCase}!`);
  }

  greetComedian({ name: "Bea Arthur" });
  ```
- 어떤 값이든 될 수 있음을 나타내려면 `unknown` 타입이 훨씬 안전합니다.

## 9.1.2 unknown

- `unkown` 타입이야말로 진정한 top 타입입니다. 모든 객체를 `unknown`에 전달할 수 있다는 점에서 `any`와 유사하지만, 타입스크립트는 `unknown` 타입의 값을 훨씬 더 제한적으로 취급한다는 점입니다.
  - 타입스크립트는 `unknown` 타입 값의 속성에 직접 접근할 수 없습니다.
  - `unknown` 타입은 top 타입이 아닌 타입에는 할당할 수 없습니다.
- 다음 코드처럼 `unknown` 속성에 직접 접근하려 하면 타입 오류를 보고합니다.
  ```tsx
  function greetComedian(name: unknown) {
    console.log(`Announcing ${name.toUpperCase()}`);
  }
  // 'name' is of type 'unknown'.
  ```
- `unknown` 타입에 직접 접근할 수 있는 방법은 `instranceof`나 `typeof` 또는 타입 어서션을 사용하는 것처럼 타입을 좁히는 경우입니다.
  ```tsx
  function greetComedianSafety(name: unknown) {
    if (typeof name === "string") {
      console.log(`Announcing ${name.toUpperCase()}`);
    } else {
      console.log("Well, I'm off.");
    }

    greetComedianSafety("Betty White");
    greetComedianSafety({});
  }
  ```
- 이런 두 가지 제한으로 인해 `unknown`이 `any`보다 훨씬 안전한 타입으로 사용되기에 가능하다면 `any` 대신 `unknown` 사용을 권장하는 편입니다.

# 9.2 타입 서술어

- `instanceof`, `typeof` 같은 자바스크립트 구문을 사용해서 type narrowing이 가능하지만 로직을 함수로 감싸면 타입을 좁힐 수 없게 됩니다.
- 아래와 같은 코드에서 우리는 if 문 내부의 값이 두 가지 타입 중 하나여야 한다고 유추할 수 있지만, 타입스크립트 내부에서는 `isNumberOrString`이 `boolean` 값을 반환한다는 사실만 알고 인수 타입을 좁히기 위함이라는 건 알 수 없기에 type error를 뱉게 됩니다.
  ```tsx
  function isNumberOrString(value: unknown) {
    return ["number", "string"].includes(typeof value);
  }

  function logValueIfExists(value: number | string | null | undefined) {
    if (isNumberOrString(value)) {
      value.toString();
      // 'value' is possibly 'null' or 'undefined'.
    } else {
      console.log("Value does not exist:", value);
    }
  }
  ```
- 인수가 특정 타입인지 여부를 나타내기 위해 `boolean` 값을 반환하는 구문을 타입 서술어(type predicate)라고 하는데, 사용자 정의 타입 가드(user-defined type guard)라고도 부릅니다.
- 타입 서술어의 반환 타입은 **매개변수의 이름, is 키워드, 특정 타입** 이렇게 선언할 수 있습니다.
  ```tsx
  function typePredicate(input: WideType): **input is NarrowType**;
  ```
- 이전 코드를 수정하면 아래와 같습니다.
  ```tsx
  function isNumberOrString(value: unknown): **value is number | string** {
      return ['number', 'string'].includes(typeof value);
  }

  function logValueIfExists(value: number | string | null | undefined) {
      if(isNumberOrString(value)) {
          value.toString();
      } else {
          console.log('Value does not exist:', value);
      }
  }
  ```
- 타입 서술어는 이미 하나의 인터페이스의 인스턴스로 알려진 객체가 더 구체적인 인터페이스 인스턴스인지 여부를 검사하는 데에 자주 사용됩니다.
  ```tsx
  interface Comedian {
    funny: boolean;
  }

  interface StandupComedian extends Comedian {
    routine: string;
  }

  function isStandupComedian(value: Comedian): value is StandupComedian {
    return "routine" in value;
  }

  function workWithComedian(value: Comedian) {
    if (isStandupComedian(value)) {
      console.log(value.routine);
    }

    console.log(value.routine);
  }
  ```
- 타입 서술어는 속성이나 값의 타입을 확인하는 것 이상을 수행하면 잘못 사용하기 쉬우므로 가능하면 피하는 것이 좋습니다. 대부분은 간단한 타입 서술어만으로도 충분합니다.

# 9.3 타입 연산자

## 9.3.1 keyof

- 자바스크립트 객체는 대부분 일반적으로 `string` 타입의 동적값을 사용해서 검색된 멤버를 갖습니다. 타입 시스템에서는 이러한 키를 표현하려면 상당히 까다로울 수 있는데, `string` 같은 포괄적 원시 타입을 사용하면 컨테이너 값에 대해 유효하지 않은 키까지 허용되게 됩니다.
  ```tsx
  interface Ratings {
      audience: number;
      critics: number;
  }

  function getRating(ratings: Ratings, key: string): number {
      return ratings[key];
  }
  // Element implicitly has an 'any' type because expression of type 'string' can't be used to index type 'Ratings'.
  //   No index signature with a parameter of type 'string' was found on type 'Ratings'.

  const ratings: Ratings = { audience: 66, critics: 84 };

  getRating(ratings, 'audience'); // Ok
  **getRating(ratings, 'not valid'); // 이런 key값이 허용되면 안 됨**
  ```
- 물론 리터럴 유니언 타입으로 제한하여 사용할 순 있습니다.
  ```tsx
  interface Ratings {
    audience: number;
    critics: number;
  }

  function getRating(ratings: Ratings, key: "audience" | "critics"): number {
    return ratings[key];
  }

  const ratings: Ratings = { audience: 66, critics: 84 };

  getRating(ratings, "audience"); // Ok
  getRating(ratings, "not valid");
  // Argument of type '"not valid"' is not assignable to parameter of type '"audience" | "critics"'.
  ```
- 하지만 `interface`의 멤버가 수십, 수백 개고 변경사항이 많다면 상당히 번거로운 작업이 되게 됩니다. 대신 타입스크립트는 해당 타입에 허용되는 모든 키의 조합을 반환하는 `keyof` 연산자를 제공합니다.
  ```tsx
  interface Ratings {
      audience: number;
      critics: number;
  }

  function getRating(ratings: Ratings, key: keyof Ratings): number {
      return ratings[key];
  }

  const ratings: Ratings = { audience: 66, critics: 84 };

  getRating(ratings, 'audience'); // Ok
  **getRating(ratings, 'not valid');**
  // Argument of type '"not valid"' is not assignable to parameter of type '"audience" | "critics"'.
  ```

## 9.3.2 typeof

- 타입스크립트에서는 `typeof` 타입 연산자로 제공되는 값의 타입을 반환합니다. 다음 예시에서 `adaptation` 변수는 `original`과 동일한 타입으로 선언되었습니다.
  ```tsx
  const original = {
    medium: "movie",
    title: "Mean Girls",
  };

  let adaptation: typeof original;

  if (Math.random() > 0.5) {
    adaptation = { ...original, medium: "play" }; // Ok
  } else {
    adaptation = { ...original, medium: 2 };
    // Type 'number' is not assignable to type 'string'.
  }
  ```
- 자바스크립트의 `typeof`와는 차이가 있습니다. 자바스크립트의 `typeof` 연산자는 해당 타입에 대한 `string`을 반환하는 런타임 연산이고, 타입스크립트의 `typeof` 연산자는 타입스크립트에서만 사용 가능하고 컴파일된 자바스크립트 코드에는 나타나지 않습니다.

### keyof typeof

- `typeof`는 값의 타입을 검색, `keyof`는 타입에 허용된 키를 검색합니다. 두 키워드를 연결해서 사용하면 값의 타입에 허용된 키를 간결하게 검색할 수 있습니다.
- 별도의 `interface`를 생성하는 대신 해당 객체에 허용된 키를 나타내는 타입에 대한 코드를 작성하는 것이 가능해집니다.
  ```tsx
  const ratings = {
    imdb: 8.4,
    metacritic: 82,
  };

  function logRating(key: keyof typeof ratings) {
    console.log(ratings[key]);
  }

  logRating("imdb");

  logRating("invalid");
  // Argument of type '"invalid"' is not assignable to parameter of type '"imdb" | "metacritic"'.
  ```

# 9.4 타입 어서션

- 타입스크립트는 코드가 강력하게 타입화(strongly typed) 될 때 가장 잘 작동합니다. 하지만 때에 따라서는 코드가 어떻게 작동되는지 타입 시스템에 100% 정확하게 알리는 게 불가능할 때도 있습니다.
- 예컨대 `JSON.parse`는 의도적으로 top 타입인 `any`를 반환합니다. `JSON.parse`를 통해 특정한 값을 반환해야 한다는 걸 타입 시스템에 알릴 방법은 없습니다.
- 타입스크립트는 해당 값의 타입에 대한 타입 시스템의 이해를 재정의하기 위해 타입 어서션(type assertion) 혹은 타입 캐스트(type cast)를 제공합니다. 해당 값 다음에 `as` 키워드를 배치합니다.
  ```tsx
  const rawData = '["grace", "frankie"]';

  // any
  JSON.parse(rawData);

  // string[]
  JSON.parse(rawData) as string[];

  // [string, string]
  JSON.parse(rawData) as [string, string];

  //  ['grace', 'frankie']
  JSON.parse(rawData) as ["grace", "frankie"];
  ```
- 하지만 가능한 완전히 타입화되어서 중간에 타입 어서션에 대한 이해 없이 코드를 이해하는 것입니다. 물론 종종 필요한 경우가 있고 유용합니다.

## 9.4.1 포착된 오류 타입 어서션

- 일반적으로 `catch` 블록에서 포착된 오류가 어떤 타입인지 아는 것은 일반적으로 불가능합니다. 코드 영역이 Error 클래스의 인스턴스를 발생시킬 거라 틀림없이 확신한다면 타입 어서션을 사용해 포착된 어서션을 오류를 처리할 수 있습니다.
  ```tsx
  try {
    // 오류 발생 코드
  } catch (error) {
    console.warn("Oh no!", (error as Error).message);
  }
  ```

## 9.4.2 non-null 어서션

- `null` 또는 `undefined`를 포함할 수 있는 변수에서 `null`과 `undefined`를 제거할 때 타입 어서션을 유용하게 사용할 수 있습니다. 타입스크립트에서는 흔한 상황이라 이왕 관련된 약어도 별도로 제공합니다. `null`과 `undefined`를 제외한 값의 전체 타입을 작성하는 대신 `!`를 사용하면 됩니다. 즉 non-null 어서션은 타입이 `null` 또는 `undefined`가 아니라고 간주합니다.
  ```tsx
  let maybeDate = Math.random() > 0.5 ? undefined : new Date();

  // 타입이 Date라고 간주됨
  maybeDate as Date;

  // 타입이 Date라고 간주됨
  maybeDate!;
  ```
- non-null 어서션은 값을 반환하거나 존재하지 않는 경우 `undefined`를 반환하는 `map.get`과 같은 API에서 특히 유용합니다.
  ```tsx
  const seasonCounts = new Map([
    ["I Love Lucy", "6"],
    ["The Golden Girls", "7"],
  ]);

  // string이나 undefined가 올 수 있음
  const maybeValue = seasonCounts.get("I Love Lucy");

  console.log(maybeValue.toUpperCase());
  // 'maybeValue' is possibly 'undefined'.
  // undefined는 toUpperCase()를 수행할 수 없음

  // string이 오도록 type narrowing
  const knowValue = seasonCounts.get("I Love Lucy")!;
  console.log(knowValue.toUpperCase());
  ```

## 9.4.3 타입 어서션 주의 사항

- `any` 타입 선언과 마찬가지로 타입 어서션은 하나의 도피 수단이 될 수 있습니다. 꼭 필요한 경우가 아니라면 사용을 줄여야 합니다. 값의 타입에 대해 어서션하는 것보다 더 정확한 타입을 갖는 것이 좋습니다.

### 어서션 vs. 선언

- 변수의 타입 애너테이션과 초깃값이 있을 때와 단순히 타입 어서션을 사용할 때는 차이가 있습니다. 타입 검사기는 변수의 초깃값에 대해 할당 가능성 검사를 수행하지만, 타입 어서션은 타입 검사 중 일부를 건너뛰도록 명시적으로 지시합니다. 그래서 하단의 코드에서 `declared` 변수는 오류를 잡을 수 있지만 `asserted` 변수에 대해서는 오류를 잡을 수 없습니다.
  ```tsx
  interface Entertainer {
    acts: string[];
    name: string;
  }

  const declared: Entertainer = {
    // Property 'acts' is missing in type '{ name: string; }' but required in type 'Entertainer'.
    name: "Moms Mabley",
  };

  const asserted = {
    name: "Moms Mabley",
  } as Entertainer; // 허용되지만 런타임 시 오류 발생

  // 런타임 시 오류 발생
  console.log(declared.acts.join(", "));
  console.log(asserted.acts.join(", "));
  ```
- 따라서 타입 애너테이션을 사용하거나 타입스크립트가 초깃값에서 변수의 타입을 유추하도록 하는 것이 좋습니다.

### 어서션 할당 가능성

- 타입스크립트는 타입 중 하나가 다른 타입에 할당 가능한 경우에만 두 타입 간의 타입 어서션을 허용합니다. 완전히 서로 관련이 없는 두 타입 사이에 타입 어서션이 있는 경우에는 타입스크립트가 타입 오류를 감지하고 알려줍니다.
  ```tsx
  let myValue = "Stella" as number; // error
  ```
- 관련 없는 타입의 전환이 필요한 경우에는 값을 `any`나 `unknown`과 같은 top 타입으로 전환한 다음 관련 없는 타입으로 전환하는 이중 타입 어서션을 사용할 순 있습니다.
  ```tsx
  let myValueDouble = "1337" as unknown as number;
  ```
- 하지만 이중 타입 어서션은 어딘가 코드의 타입이 잘못되었다는 징후이기도 하고, 예측 가능성을 떨어뜨리기 때문에 위험한 선언이라고 볼 수 있습니다.

# const 어서션

## 9.5.1 리터럴에서 원시 타입으로

- 타입 시스템이 리터럴 값을 일반적인 원시 타입으로 확장하기보다 특정 리터럴로 이해하는 것이 유용할 수 있습니다. 다음과 같이 좀 더 구체적인 값을 반환하도록 할 수 있습니다.
  ```tsx
  // 타입: () => string
  const getName = () => {
    "Maria Bamford";
  };

  // 타입: () => "Maria Bamford"
  const getNameConst = () => {
    "Maria Bamford" as const;
  };
  ```
- 값의 특정 필드가 더 구체적인 리터럴 값을 갖도록 하는 것에도 유용합니다. 다음 `narrowJoke` 변수는 `string` 대신 “one-liner”라는 특정 값을 가지므로 `Joke` 타입이 필요한 위치에 제공될 수 있습니다.
  ```tsx
  interface Joke {
    quote: string;
    style: "story" | "one-liner";
  }

  function tellJoke(joke: Joke) {
    if (joke.style === "one-liner") {
      console.log(joke.quote);
    } else {
      console.log(joke.quote.split("\n"));
    }
  }

  const narrowJoke = {
    quote: "If you stay alive for no other reason do it for spite.",
    style: "one-liner" as const,
  };

  tellJoke(narrowJoke);

  const wideObject = {
    quote: "Time files when you are anxious!",
    style: "one-liner",
  };

  tellJoke(wideObject);
  // Argument of type '{ quote: string; style: string; }' is not assignable to parameter of type 'Joke'.
  // Types of property 'style' are incompatible.
  // Type 'string' is not assignable to type '"story" | "one-liner"'.
  ```

## 9.5.2 읽기 전용 객체

- 특정 리터럴이 필요한 위치에서 사용해야할 때는 `as const`를 사용해 값 리터럴을 어서션하면 유추된 타입이 가능한 한 구체적으로 전환됩니다. 모든 멤버 속성은 `readonly`가 되고, 리터럴은 일반적인 원시 타입 대신 고유한 리터럴 타입으로 간주되며, 배열은 읽기 전용 튜플이 됩니다. 즉, 값 리터럴에 `const` 어서션을 적용하면 해당 값 리터럴이 변경되지 않고 모든 멤버에 동일한 `const` 어서션 로직이 재귀적으로 적용됩니다.
