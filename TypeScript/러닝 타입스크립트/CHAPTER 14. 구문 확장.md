# CHAPTER 14. 구문 확장

- 오늘날에는 타입스크립트와 같은 상위 집합 언어에 특정 새로운 런타임 기능으로 자바스크립트 구문을 확장하는 방식은 나쁜 사례로 간주하고 권장되지 않습니다. 단, 일부 프로젝트에서 여전히 사용되기도 하는 일부 구문 확장이 있고, 타입스크립트는 데코레이터(decorator)에 대한 실험적 제안을 채택했습니다.

# 14.1 클래스 매개변수 속성

```tsx
TIP. 클래스를 많이 사용하는 프로젝트나 클래스 이점을 갖는 프레임워크가 아니라면 클래스 매개변수 속성을 사용하지 않는 것이 좋습니다.
```

- 다음 Engineer 클래스는 타입이 string인 하나의 area 매개변수를 받고, 타입이 string인 area 클래스 변수에 할당합니다.
  ```tsx
  class Enginner {
    readonly area: string;

    constructor(area: string) {
      this.area = area;
      console.log(`I work in the ${area} area.`);
    }
  }

  new Enginner("mechanical").area;
  ```
- 다음과 같은 단축 구문으로 재작성할 수 있습니다.
  ```tsx
  class Enginner {
    constructor(readonly area: string) {
      console.log(`I work in the ${area} area.`);
    }
  }

  new Enginner("mechanical").area;
  ```

# 14.2 실험적인 데코레이터

```tsx
TIP. ECMA스크립트 버전이 데코레이터 구문으로 승인될 때까지 가능하면 데코레이터를 사용하지 않는 것이 좋습니다. 타입스크립트 데코레이터 사용을 권장하는 앵귤러나 Nest.js와 같은 프레임워크는 공식 문서에서 사용 방법을 안내합니다.
```

- TSConfig를 통해 experimetalDecorators 옵션을 활성화할 수 있습니다.
- 데코레이터의 각 사용법은 데코레이팅하는 엔티티, 즉 데코레이터가 적용되는 함수 등이 생성되자마자 한 번 실행됩니다.

# 14.3 열거형

```tsx
TIP. 자주 반복되는 리터럴 집합이 있고, 그 리터럴 집합을 공통 이름으로 설명할 수 있으며, 열거형으로 전환했을 때 훨씬 더 읽기 쉽지 않은 경우라면 열거형을 사용해서는 안 됩니다.
```

- 자바스크립트는 열거형 구문을 포함하지 않으므로 일반적으로 const 객체를 사용합니다.
  ```tsx
  const StatusCodes = {
    InternalServerError: 500,
    NotFound: 404,
    Ok: 200,
  } as const;

  StatusCodes.InternalServerError;
  ```

## 14.3.1 자동 숫잣값

- 열거형의 멤버는 명시적인 초깃값을 가질 필요가 없습니다. 값이 생략되면 타입스크립트는 첫 번째 값을 0으로 시작하고 각 후속 값을 1씩 증가합니다.
  ```tsx
  enum VisualTheme {
    Dark, // 0
    Light, // 1
    System, // 2
  }
  ```
  ```jsx
  "use strict";
  var VisualTheme;
  (function (VisualTheme) {
    VisualTheme[(VisualTheme["Dark"] = 0)] = "Dark";
    VisualTheme[(VisualTheme["Light"] = 1)] = "Light";
    VisualTheme[(VisualTheme["System"] = 2)] = "System";
  })(VisualTheme || (VisualTheme = {}));
  ```
- Top 멤버의 값이 명시적으로 1로 선언되면 나머지 값은 양의 정수로 1씩 증가합니다.
  ```tsx
  enum Direction {
    Top = 1,
    Right,
    Bottom,
    Left,
  }
  ```
  ```jsx
  "use strict";
  var Direction;
  (function (Direction) {
    Direction[(Direction["Top"] = 1)] = "Top";
    Direction[(Direction["Right"] = 2)] = "Right";
    Direction[(Direction["Bottom"] = 3)] = "Bottom";
    Direction[(Direction["Left"] = 4)] = "Left";
  })(Direction || (Direction = {}));
  ```

```
WARNING. 열거형의 순서를 수정하면 기본 번호가 변경되기 때문에 수정이나 항목을 제거할 때는 주의해야 합니다.
```

## 14.3.2 문자열값을 갖는 열거형

- 숫자 대신 문자열값을 사용할 수 있습니다.
  ```tsx
  enum LoadStyle {
    AsNeeded = "as-needed",
    Eager = "eager",
  }
  ```
  ```jsx
  "use strict";
  var LoadStyle;
  (function (LoadStyle) {
    LoadStyle["AsNeeded"] = "as-needed";
    LoadStyle["Eager"] = "eager";
  })(LoadStyle || (LoadStyle = {}));
  ```
- 문자열 값의 단점은 자동 숫잣값처럼 자동으로 계산할 수 없다는 점입니다. 그렇다고 숫자와 문자열 모두 멤버로 갖는 열거형은 보통 불필요하고 혼란스럽기 때문에 사용하진 않습니다.

## 14.3.3 const 열거형

- 열거형은 런타임 객체를 생성하므로 리터럴 값 유니언을 사용하는 일반적인 전략보다 더 많은 코드를 생성하게 됩니다. const 제한자로 열거형을 선언하면 컴파일된 자바스크립트에서 객체 정의와 속성 조회를 생략하도록 지시합니다.
  ```tsx
  const enum DisplayHint {
    Opaque = 0,
    Semitransparent,
    Transparent,
  }

  let displayHint = DisplayHint.Transparent;
  ```
  ```jsx
  "use strict";
  let displayHint = 2; /* DisplayHint.Transparent */
  ```

# 14.4 네임스페이스

```jsx
WARNING. 기존 패키지에 대한 DefinitelyTyped 타입 정의를 작성하지 않는 한 네임스페이스를 사용하지 마세요.최신 자바스크립트 모듈 의미 체계와 일치하지 않습니다.
```

- 추후 필요 시 체크 예정

# 14.5 타입 전용 가져오기와 내보내기

- 이 기능은 크게 필요 없을 수도 있지만, `--isolatedModules`, TypeScript의 `transpileModule` API, 또는 Babel에서 문제가 발생하면 이 기능과 관련이 있을 수 있습니다. 타입 전용 import ,export 별도의 구문이 존재합니다.
- 여기서 중요한 점이 interface는 Type-only export를 사용할 수가 없다는 점입니다. 오직 type alias를 사용했을 때만 Type-only export를 사용할 수가 있습니다.
- 필수적으로 선언이 필요하진 않지만 코드 가독성을 향상시키고, 불필요한 모듈 로딩 방지 및 컴파일 속도 향상 등 장점이 존재합니다.
  ```tsx
  import { type TypeOne, value } from "my-example-types";
  import type { TypeTwo } from "my-example-types";
  import type DefaultType from "my-example-types";

  export { type TypeOne, value };
  export type { DefaultType, TypeTwo };
  ```
  ```jsx
  import { value } from "my-example-types";
  export { value };
  ```

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)
- https://www.typescriptlang.org/ko/docs/handbook/enums.html
- https://www.typescriptlang.org/ko/docs/handbook/release-notes/typescript-3-8.html
- https://kagrin97-blog.vercel.app/types/TypeOnly-Import,Export
