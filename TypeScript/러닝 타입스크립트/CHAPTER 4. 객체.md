# PART 1. 개념

## CHAPTER 4. 객체

### 객체 타입

- 객체 타입 선언 - 객체의 타입을 명시적으로 선언하고 싶을 때 아래와 같이 선언할 수 있습니다.

    ```tsx
    let poetLater: {
        born: number;
        name: string;
    }
    
    poetLater = {
        born: 1935,
        name: 'Mary Oliver',
    }
    
    poetLater = "Sappho"; // Type 'string' is not assignable to type '{ born: number; name: string; }'.
    ```

- 단, 저런 형식으로 매번 객체 타입을 작성하는 것은 번거로운 일입니다. 그래서 타입 별칭을 사용하는 것이 좀 더 일반적입니다.

    ```tsx
    type Poet = {
        born: number;
        name: string;
    }
    
    let poetLater: Poet;
    
    poetLater = {
        born: 1935,
        name: "Sara Teasdale",
    }
    
    poetLater = "Emily Dickinson"; // Type 'string' is not assignable to type 'Poet'.
    ```

  - 대부분의 타입스크립트 프로젝트는 객체 타입을 설명할 때 인터페이스(interface) 키워드 사용을 선호합니다. 별칭 객체 타입과 거의 동일합니다.

### 구조적 타이핑

- 자바스크립트는 **덕 타입(duck typed)**인 반면, 타입스크립트는 **구조적으로 타입화**됩니다.
- 사용 검사
  - 할당하는 값에는 객체 타입의 필수 속성이 있어야 합니다. 객체 타입에 필요한 멤버가 없다면 타입 오류가 발생합니다.

      ```tsx
      type FirstAndLastNames = {
          first: string;
          last: string;
      }
      
      // Ok
      const hasBoth: FirstAndLastNames = {
          first: 'Sarojini',
          last: 'Naidu',
      }
      
      // Property 'last' is missing in type '{ first: string; }'
      // but required in type 'FirstAndLastNames'.
      const hasOnlyOne: FirstAndLastNames = {
          first: "Sappho"
      }
      ```

  - 또한 당연히 지정된 객체의 속성과 일치하지 않으면 타입 오류를 발생시킵니다.
- 초과 속성 검사
  - 초깃값에 객체 타입에서 정의된 것보다 많은 필드가 있으면 타입 오류가 발생합니다.
  - 초과 속성 검사는 **객체 타입으로 선언된 위치에서 생성되는 객체 리터럴에 대해서만** 일어납니다.

      ```tsx
      type Poet = {
          born: number;
          name: string;
      }
      
      const poetMatch: Poet = {
          born: 1928,
          name: "Maya Angelou";
      };
      
      const extraProperty: Poet = {
          activity: "walking",
          // Object literal may only specify known properties, and 'activity' does not exist in type 'Poet'.
          born: 1935,
          name: "Mary Oliver"
      }
      ```

  - 기존 객체 리티럴을 제공하면 초과 속성 검사를 우회합니다.

      ```tsx
      type Poet = {
          born: number;
          name: string;
      }
      
      const existingObject = {
          activity: "walking",
          born: 1935,
          name: "Mary Oliver",
      }
      
      const extraPropertyButOk: Poet = existingObject;
      ```

  - 중첩된 객체 타입 - 중첩된 타입을 자체 타입 별칭으로 추출하면 타입스크립트의 타입 오류 메시지에 더 많은 정보를 담을 수 있습니다.
  - 선택적 속성 - 타입의 속성 애너테이션에서 : 앞에 `?`를 추가하면 선택적 속성임을 나타낼 수 있습니다. **유니언 타입과의 차이점이라면, 유니언 타입은 둘 중에 어느 하나의 타입은 반드시 있어야 하지만 선택적 속성은 해당 속성이 존재하지 않아도 됩니다.**

      ```tsx
      type Book = {
          author?: string;
          pages: number;
      }
      
      const ok: Book = {
          author: "Rita Dove",
          pages: 80,
      }
      
      const missing: Book = {
          pages: 100,
      }
      ```


### 객체 타입 유니언

- 유추된 객체 타입 유니언 - 변수에 여러 객체 타입 중 하나가 될 수 있는 초깃값이 주어지면 타입스크립트는 해당 타입을 객체 타입 유니언으로 유추합니다.

    ```tsx
    const poem = Math.random() > 0.5
        ? { name: "The Double Image", pages: 7 }
        : { name: "Her Kind", rhymes: true };
    // 타입:
    // {
    //     name: string;
    //     pages: number;
    //     rhymes?: undefined;
    // }
    // |
    // {
    //     name: string;
    //     pages?: undefined;
    //     rhymes: boolean;
    // }
    ```

- 명시된 객체 타입 유니언 - 명시적으로 선언을 해두면 모든 유니언 타입에 존재하는 속성에 대한 접근만 허용합니다. 잠재적으로 존재하지 않는 객체의 멤버에 대한 접근을 제한할 수 있기 때문에 객체 타입 유니언도 타입을 좁혀서 좀 더 안전하게 작성할 수 있습니다.

    ```tsx
    type PoemWithPages = {
        name: string;
        pages: number;
    }
    
    type PoemWithRhymes = {
        name: string;
        rhymes: boolean;
    }
    
    type Poem = PoemWithPages | PoemWithRhymes;
    
    const poem: Poem = Math.random() > 0.5
        ? { name: "The Double Image", pages: 7 }
        : { name: "Her Kind", rhymes: true };
    
    poem.name;
    
    poem.pages;
    // Property 'pages' does not exist on type 'Poem'.
    //   Property 'pages' does not exist on type 'PoemWithRhymes'.
    ```

- 객체 타입 내로잉 - 타입 내로잉을 통해서 해당 타입의 범위를 명시적으로 좁힐 수 있습니다.

    ```tsx
    type PoemWithPages = {
        name: string;
        pages: number;
    }
    
    type PoemWithRhymes = {
        name: string;
        rhymes: boolean;
    }
    
    type Poem = PoemWithPages | PoemWithRhymes;
    
    const poem: Poem = Math.random() > 0.5
        ? { name: "The Double Image", pages: 7 }
        : { name: "Her Kind", rhymes: true };
    
    poem.name;
    
    if("pages" in poem) {
        poem.pages;
    }
    ```

- 판별된 유니언(discriminated union) - 각각의 타입에 타입을 가리키는 속성, 즉 판별값을 부여하고, 타입 내로잉을 수행할 수 있습니다.

    ```tsx
    interface Bird {
      type: "bird";
      flyingSpeed: number;
    }
    
    interface Horse {
      type: "horse";
      runningSpeed: number;
    }
    
    type Animal = Bird | Horse;
    
    function getAnimalSpped(animal: Animal) {
      let speed: number = 0;
      switch (animal.type) {
        case "bird":
          speed = animal.flyingSpeed;
          break;
        case "horse":
          speed = animal.runningSpeed;
          break;
      }
      console.log(`${animal.type} is moving with speed ${speed}km`);
    }
    
    getAnimalSpped({
      type: "bird",
      flyingSpeed: 10,
    });
    ```


### 교차 타입

- 자바스크립트의 런타임 `|` 연산자가 `&` 연산자에 대응하는 역할을 하는 것처럼, 타입스크립트에서도 `&` 교체 타입(intersection type)을 사용해 여러 타입을 동시에 나타냅니다.

    ```tsx
    type Artwork = {
        genre: string;
        name: string;
    };
    
    type Writing = {
        pages: number;
        name: string;
    }
    
    type WrittenArt = Artwork & Writing;
    
    // 다음과 같음
    // {
    //     genre: string;
    //     name: string;
    //     pages: number;
    // }
    ```

- 교차 타입의 위험성 - 유용한 개념이지만, 혼동될 수 있기 때문에 가능한 코드를 간결하게 유지해야 합니다.
  - 긴 할당 가능성 오류 - 복잡한 교차 타입을 만들게 되면 할당 가능성 오류 메시지는 읽기 어려워집니다.
  - `never` - 원시 타입의 값은 동시에 여러 타입이 될 수 없기 때문에 교차 타입의 구성 요소로 함께 결합할 수 없습니다. 두 개의 원시 타입을 함께 시도하면 `never` 키워드로 표시되는 `never` 타입이 됩니다.

      ```tsx
      type NotPossible = number & string; // 타입: never
      ```

    - `never` 키워드와 `never` 타입은 프로그래밍 언어에서 bottom 타입 또는 empty 타입을 뜻합니다. bottom 타입은 값을 가질 수 없고 참조할 수 없는 타입이므로 bottom 타입에 그 어떠한 타입도 제공할 수 없습니다.

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)
- [https://velog.io/@hoplin/TypeScript-고급타입-구별된-유니온Discriminated-Union](https://velog.io/@hoplin/TypeScript-%EA%B3%A0%EA%B8%89%ED%83%80%EC%9E%85-%EA%B5%AC%EB%B3%84%EB%90%9C-%EC%9C%A0%EB%8B%88%EC%98%A8Discriminated-Union)