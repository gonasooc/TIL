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