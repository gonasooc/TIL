# PART 2. 특징

## CHAPTER 6. 배열

- 자바스크립트의 배열은 매우 유연하고 내부의 모든 타입의 값을 혼합해서 저장할 수 있습니다. 다만 대부분의 배열은 하나의 특정 타입의 값만 가집니다. 다른 타입의 값을 추가하게 되면 혼란을 줄 수 있고, 최악의 경우 오류가 생길 수 있습니다.
- 타입스크립트는 초기 배열에 어떤 데이터 타입이 있는지 기억하고, 해당 데이터 타입에서만 작동하도록 제한하는 방식으로 데이터 타입을 하나로 유지합니다.

    ```tsx
    const warriors = ["Artemisia", "Boudica"];
    
    warriors.push("Zenobia");
    
    warriors.push(true);
    // Argument of type 'boolean' is not assignable to parameter of type 'string'.
    ```


### 배열 타입

- 배열과 함수 타입 - 배열 타입은 함수 타입에 무엇이 있는지를 구별하는 괄호가 필요한 구문 컨테이너의 예입니다. 괄호는 애너테이션의 어느 부분이 함수 반환 부분이고, 어느 부분이 배열 타입 묶음인지를 나타내기 위해 사용합니다.

    ```tsx
    // 타입은 string 배열을 반환하는 함수
    let createStrings: () => string[];
    
    // 타입은 각각의 string을 반환하는 함수 배열
    let stringCreators: (() => string)[];
    ```

- 유니언 타입 배열 - 애너테이션의 어느 부분이 배열의 컨텐츠고 어느 부분이 유니언 타입 묶음인지를 나타내기 위해 괄호를 사용해야 할 수도 있습니다.

    ```tsx
    // 타입은 string 또는 number의 배열
    let stringOrArrayOfNumbers: string | number[];
    
    // 타입은 각각 number 또는 string인 요소의 배열
    let arrayOfStringOrNumbers: (string | number)[];
    ```

- any 타입의 진화 - 타입을 명확하게 선언해주지 않으면 아래 예시처럼 type 변경이 생기게 됩니다.

    ```tsx
    // 타입: any[]
    let values = [];
    
    // 타입: string[]
    values.push('');
    
    // 타입: (number | string)[]
    values[0] = 0;
    ```

- 다차원 배열 - 2차원 배열 또는 배열의 배열은 두 개의 `[]`(대괄호)를 갖습니다.

    ```tsx
    let arrayOfArraysOfNumbers: number[][];
    
    arrayOfArraysOfNumbers = [
        [1, 2, 3],
        [2, 4, 6],
        [3, 6, 9]
    ]
    ```


### 배열 멤버

- 타입스크립트는 배열의 멤버를 찾아서 해당 배열의 타입 요소를 되돌려주는 전형적인 인덱스 기반 접근 방식의 언어입니다.

    ```tsx
    const defenders = ["Clarenza", "Dina"];
    
    // 타입: string
    const defender = defenders[0];
    ```

- 유니언 타입으로 된 배열의 멤버는 동일하게 유니언 타입입니다.

    ```tsx
    const soldiersOrDates = ["Deborah Sampson", new Date(1782, 6, 3)];
    
    // 타입: string | Date
    const soldierOrDate = soldiersOrDates[0];
    ```

- 주의 사항: 불안정한 멤버 - 배열은 타입 시스템에서 불안정한 소스입니다.

    ```tsx
    function withElements(elements: string[]) {
        console.log(elements[9001].length); // 타입 오류 없음
    }
    
    withElements(["It's", "over"]);
    ```

  - 배열 조회를 더 제한하고 타입을 안전하게 만드는 `noUncheckedIndexedAccess` 플래그가 있지만 매우 엄격해서 주로 사용되진 않습니다.

### 스프레드와 나머지 매개변수

- 스프레드 - …스프레드 연산자를 사용해 배열을 결합합니다. 입력 배열이 동일한 타입이라면 출력 배열도 동일한 타입입니다. 두 배열을 결합하면 새 배열은 두 개의 원래 타입 중 어느 하나의 요소인 유니언 타입 배열로 이해하게 됩니다.

    ```tsx
    // 타입: string[]
    const soldiers = ["Harriet Tubman", "Joan of Arc", "Khutulun"];
    
    // 타입: number[]
    const soldierAges = [90, 19, 45];
    
    // 타입: (string | number)[]
    const conjoined = [...soldiers, ...soldierAges];
    ```

- 나머지 매개변수 스프레드 - 나머지 매개변수 또한 타입 검사를 수행합니다. 인수로 사용되는 배열은 지정된 타입과 동일한 배열 타입을 가져야 합니다.

    ```
    function logWarries(greeting: string, ...names: string[]) {
        for(const name of names) {
            console.log(`${greeting}, ${name}!`);
        }
    }
    
    const warriors = ["Cathay Williams", "Lozen", "Nzinga"];
    
    logWarries("Hello", ...warriors);
    
    const birthYears = [1822, 1923, 2001];
    
    logWarries("Born in", ...birthYears);
    // Argument of type 'number' is not assignable to parameter of type 'string'.
    ```


### 튜플

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)