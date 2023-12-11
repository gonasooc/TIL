# PART 1. 개념

## CHAPTER 3. 유니언과 리터럴

- 타입스크립트가 해당 값을 바탕으로 추론을 수행하는 두 가지 핵심 개념
  - 유니언(union): 값에 허용된 타입을 두 개 이상의 가능한 타입으로 확장하는 것
  - 내로잉(narrowing): 값에 허용된 타입이 하나 이상의 가능한 타입이 되지 않도록 좁히는 것

### 유니언 타입

- 변수의 초깃값이 있더라도 변수에 대한 명시적 타입 애너테이션을 제공하는 것이 유용할 때 유니언 타입을 사용합니다.

    ```tsx
    let thinker: string | null = null; // 초깃값은 null이지만,
    
    if(Math.random() > 0.5) {
    	thinker = "Susanne Langer"; // null 대신 string이 될 수 있음
    }
    ```

- 유니언 타입 선언의 순서는 중요하지 않습니다. 타입스크립트에서는 `boolean | number`나 `number | boolean` 모두 똑같이 취급합니다.
- 유니언 타입일 때 유니언으로 선언한 모든 가능한 타입에 존재하는 멤버 속성에만 접근할 수 있습니다. 이건 일종의 안전 조치에 해당합니다.

    ```tsx
    let physicist = Math.random() > 0.5 ? "Marie Curie" : 84;
    
    physicist.toString();
    physicist.toUpperCase(); // physicist.toUpperCase is not a function 
    physicist.toFixed(); // physicist.toUpperCase is not a function
    ```


### 내로잉

- 유니언 타입에서 하나의 타입으로 된 값의 속성을 사용하려면 구체적인 타입(specific type)을 선언해줘야 하는데, 이때 사용하는 것이 **내로잉**입니다.
- 변수에 유니언 타입 애너테이션이 명시되고 초깃값이 주어질 때 **값 할당 내로잉**이 작동합니다. 타입스크립트는 유니언 타입 중 하나의 값을 받을 수 있지만,초깃값의 타입으로 시작합니다.

    ```tsx
    let admiral: number | string;
    
    admiral = "Grace Hopper";
    
    admiral.toUpperCase(); // Ok: string
    admiral.toFixed(); // admiral.toFixed is not a function
    ```

- 단, 일반적으로 if문을 통한 내로잉을 주로 사용합니다.

    ```tsx
    let scientist = Math.random() > 0.5 ? "Rosalind Franklin" : 51;
    
    if (scientist === "Rosalind Franklin") {
      scientist.toUpperCase(); // OK
    }
    
    scientist.toUpperCase(); // [ERR]: scientist.toUpperCase is not a function
    ```

- `typeof` 검사를 통한 내로잉도 가능합니다. `typeof` 검사는 타입을 좁히기 위해 **자주 사용하는 실용적인 방법**입니다.

    ```tsx
    let researcher = Math.random() > 0.5 ? "Rosalind Franklin" : 51;
    
    if(typeof researcher === 'string') {
      researcher.toUpperCase(); // Ok: string
    }
    ```


### 리터럴 타입

- 리터럴 타입은 좀 더 구체적인 버전의 원시타입입니다. 원시 타입 값 중 어떤 것이 아닌 **특정 원싯값**으로 알려진 타입이 리터럴 타입입니다. 즉, 원시 타입은 해당 타입의 가능한 모든 리터럴 값의 집합이라고 볼 수 있습니다.
- 리터럴 타입을 선언한 값에는 다른 값이나 다른 타입을 할당할 수 없습니다. 단, 리터럴 타입 그 자체는 그 값이 해당하는 원시 타입에는 할당할 수 있습니다.

    ```tsx
    let specificallyAda: "Ada";
    
    specificallyAda = "Ada"; // OK
    specificallyAda = "Byron"; // Type '"Byron"' is not assignable to type '"Ada"'.
    
    let someString = "";
    specificallyAda = someString; // Type 'string' is not assignable to type '"Ada"'
    
    someString = specificallyAda; // Ok
    ```


### 엄격한 null 검사

- 리터럴로 좁혀진 유니언의 힘은 타입스크립트에서 **엄격한 null 검사(strict null checking)**라 부르는 타입 시스템 영역인 ‘잠재적으로 정의되지 않은 `undefined` 값’으로 작업할 때 특히 두드러집니다. `strictNullChecks`는 엄격한 `null` 검사를 활성화할지 여부를 결정합니다. `strictNullChecks`를 활성화하면 코드가 `null` 또는 `undefined` 값으로 인한 오류로부터 안전한지 여부를 쉽게 파악할 수 있습니다.

    ```tsx
    let nameMaybe = Math.random() > 0.5 ? "Tony Hoare" : undefined;
    
    nameMaybe.toLowerCase(); // nameMaybe' is possibly 'undefined'.
    ```

- 참 검사를 통한 내로잉도 가능합니다. 자바스크립트에서 `false`, `0`, `-0`, `0n`, `“”`, `null`, `undefined`, `NaN`처럼 **falsy**로 정의된 값을 제외한 모든 값은 모두 참입니다. 잠재적인 값 중 **truthy**로 확인된 일부에 한해서만 변수의 타입을 좁힐 수 있습니다.

    ```tsx
    let geneticist = Math.random() > 0.5 ? "Barbara McClintock" : undefined;
    
    if(geneticist) {
      geneticist.toUpperCase();
    }
    
    geneticist.toUpperCase(); // 'geneticist' is possibly 'undefined'.
    ```


### 타입 별칭

- 타입스크립트에는 재사용하는 타입에 더 쉬운 이름을 할당하는 **타입 별칭(type alias)**이 있습니다. 타입 별칭은 파스칼 케이스(PascalCase)로 지정합니다.

    ```tsx
    type RawData = boolean | number | string | null | undefined;
    
    let rawDataFirst: RawData;
    let rawDataSecond: RawData;
    let rawDataThird: RawData;
    ```

- 타입 애너테이션과 마찬가지로 자바스크립트로 컴파일되진 않습니다. 런타임에 존재하지 않는 항목이기 때문에 접근하려 하면 타입 오류가 나옵니다. 타입 별칭은 순전히 ‘개발 시’에만 존재합니다.

    ```tsx
    type SomeType = string | undefined;
    console.log(SomeType); // 'SomeType' only refers to a type, but is being used as a value here.
    ```

- 타입 별칭은 다른 타입 별칭을 참조해서 작성할 수 있습니다.

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)