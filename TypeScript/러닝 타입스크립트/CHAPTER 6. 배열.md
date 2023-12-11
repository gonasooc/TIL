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

- 배열과 함수 타입

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)