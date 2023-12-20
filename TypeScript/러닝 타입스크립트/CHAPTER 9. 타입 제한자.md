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
        console.log(`Announcing ${name.toUpperCase()}`)
    }
    // 'name' is of type 'unknown'.
    ```

- `unknown` 타입에 직접 접근할 수 있는 방법은 `instranceof`나 `typeof` 또는 타입 어서션을 사용하는 것처럼 타입을 좁히는 경우입니다.

    ```tsx
    function greetComedianSafety (name: unknown) {
        if (typeof name === "string") {
            console.log(`Announcing ${name.toUpperCase()}`)
        } else {
            console.log("Well, I'm off.");
        }
        
        greetComedianSafety("Betty White");
        greetComedianSafety({});
    }
    ```

- 이런 두 가지 제한으로 인해 `unknown`이 `any`보다 훨씬 안전한 타입으로 사용되기에 가능하다면 `any` 대신 `unknown` 사용을 권장하는 편입니다.

# 9.2 타입 서술어

- instanceof, typeof 같은 자바스크립트 구문을 사용해서 type narrowing이 가능하지만 로직을 함수로 감싸면 타입을 좁힐 수 없게 됩니다.
- 아래와 같은 코드에서 우리는 if 문 내부의 값이 두 가지 타입 중 하나여야 한다고 유추할 수 있지만, 타입스크립트 내부에서는 isNumberOrString이 boolean 값을 반환한다는 사실만 알고 인수 타입을 좁히기 위함이라는 건 알 수 없기에 type error를 뱉게 됩니다.

    ```tsx
    function isNumberOrString(value: unknown) {
        return ['number', 'string'].includes(typeof value);
    }
    
    function logValueIfExists(value: number | string | null | undefined) {
        if(isNumberOrString(value)) {
            value.toString();
    				// 'value' is possibly 'null' or 'undefined'.
        } else {
            console.log('Value does not exist:', value);
        }
    }
    ```

- 인수가 특정 타입인지 여부를 나타내기 위해 boolean 값을 반환하는 구문을 타입 서술어(type predicate)라고 하는데, 사용자 정의 타입 가드(user-defined type guard)라고도 부릅니다.
- 타입 서술어의 반환 타입은 **매개변수의 이름, is 키워드, 특정 타입** 이렇게 선언할 수 있습니다.

    ```tsx
    function typePredicate(input: WideType): input is NarrowType;
    ```

- 이전 코드를 수정하면 아래와 같습니다.