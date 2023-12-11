# PART 2. 특징

## CHAPTER 5. 함수

### 함수 매개변수

- 필수 매개변수 - 자바스크립트에서는 인수의 수와 상관없이 함수를 호출할 수 있습니다. 잘못된 수의 인수로 호출되면 오류 가능성이 있기 때문에 이의를 제기합니다. 필수 매개변수를 제공하고 강제하면 타입 안정성을 강화하는 데 도움이 됩니다.

    ```tsx
    function singTwo(first: string, second: string) {
        console.log(`${first} / ${second}`);
    }
    
    singTwo("Ball and Chain"); // Expected 2 arguments, but got 1.
    singTwo("Ball and Chain", "Higher Love");
    singTwo("Ball and Chain", "Higher Love", "Dreams"); // Expected 2 arguments, but got 3.
    ```

- 선택적 매개변수 - 자바스크립트에서 함수의 매개변수가 제공되지 않으면 함수 내부의 인숫값은 `undefined`가 기본값으로 설정됩니다. 물론 `undefined` 값을 위해 의도적으로 사용할 수도 있습니다. 타입 애너테이션 `:` 앞에 `?`를 추가하면 매개변수가 선택적이라는 것으로 지정할 수 있습니다. 유니언 타입 매개변수와는 다릅니다. 유니언 매개변수는 `undefined`일지라도 명시적으로 항상 제공되어야 합니다.

    ```tsx
    function announceSongBy(song: string, singer: string | undefined) {
        console.log('test');
    }
    
    announceSongBy('Greensleeves'); // Expected 2 arguments, but got 1.
    announceSongBy('Greensleeves', undefined);
    announceSongBy('Greensleeves', 'Sia');
    ```

- 기본 매개변수 - 자바스크립트에서 매개변수 선언 시 `=`와 같이 기본값을 포함할 수 있습니다. 선택적 매개변수에는 기본적으로 값이 제공되기 때문에 해당 타입스크립트 타입에는 암묵적으로 함수 내부에 `| undefined` 유니언 타입이 추가됩니다. 타입 스크립트의 타입 추론은 초기 변숫값과 마찬가지로 기본 함수 매개변수에 대해서도 유사하게 작동합니다. 매개변수에 기본값이 있고 타입 애너테이션이 없는 경우, 해당 기본값을 기반으로 타입을 유추합니다.

    ```tsx
    function rateSong(song: string, rating = 0) {
        console.log(`${song} gets ${rating}/5 stars!`);
    }
    
    rateSong("Photograph");
    rateSong("Set Fire to tje Rain", 5);
    rateSong("Set Fire to tje Rain", undefined);
    
    rateSong("At Last!", "100");
    // Argument of type 'string' is not assignable to parameter of type 'number'.
    ```

- 나머지 매개변수(rest parameter) - 자바스크립트의 일부 함수는 임의의 수의 인수로 호출할 수 있도록 만들어집니다. …스프레드 연산자는 함수 선언의 마지막 매개변수에 위치하고, 해당 매개변수에서 시작해 함수의 전달된 ‘나머지(rest)’ 인수가 모두 단일 배열에 저장되어야 함을 나타냅니다. 타입스크립트에서 애너테이션 끝에 [] 구문이 추가되어 표기합니다.

    ```tsx
    function singAllTheSongs(singer: string, ...songs: string[]) {
        for(const song of songs) {
            console.log(`${song}, by ${singer}`);
        }
    }
    
    singAllTheSongs("Alicia Keys");
    singAllTheSongs("Lady Gaga", "Bad Romance", "Just Dance", "Poker Face");
    
    singAllTheSongs("Ella Fitzgerald", 2000);
    // Argument of type 'number' is not assignable to parameter of type 'string'.
    ```


### 반환 타입

- 함수에 여러 개의 반환문을 포함하고 있으면, 타입스크립트는 반환 타입(return type)을 가능한 모든 반환 타입의 조합으로 유추합니다. 함수의 반환 타입 애너테이션은 매개변수 목록이 끝나는 `)` 다음에 배치됩니다.
- 명시적 반환 타입 - 함수의 반환문이 함수의 반환 타입으로 할당할 수 없는 값을 반환하는 경우 타입스크립트는 할당 가능성 오류를 표시합니다.

    ```tsx
    function getSongRecordingDate(song: string): Date | undefined {
        switch(song) {
            case "Strange Fruit":
                return new Date('April 20, 1999'); // Ok
    
            case "Greensleeves";
                return "unknown";
                // Type 'string' is not assignable to type 'Date'.
            
            default:
                return undefined;
        }
    }
    ```


### 함수 타입

- 자바스크립트에서는 일급 객체(First-class object)로 취급되기 때문에 함수를 변수에 값으로 전달할 수 있습니다. 함수 타입(function type) 구문은 화살표 함수와 유사하지만 함수 본문 대신 타입이 있습니다.
- 함수 타입 괄호 - 함수 타입은 다른 타입이 사용되는 모든 곳에 배치할 수 있습니다. 여기에는 유니언 타입도 포함됩니다.

    ```tsx
    // 타입은 string | undefined 유니언을 반환하는 함수
    let returnsStringOrUndefined: () => string | undefined;
    
    // 타입은 undefined나 string을 반환하는 함수
    let maybeReturnsString: (() => string) | undefined;
    ```

- 매개변수 타입 추론 - 타입스크립트는 선언된 타입의 위치에 제공된 함수의 매개변수 타입을 유추할 수 있습니다.

    ```tsx
    let singer: (song: string) => string;
    
    singer = function (song) {
        // song: string의 타입
        return `Singing: ${song.toUpperCase()}!`; // Ok
    }
    ```

- 함수 타입 별칭 - 함수 타입에도 동일하게 별칭을 사용할 수 있습니다.

    ```tsx
    type StringToNumber = (input: string) => number;
    
    let StringToNumber: StringToNumber;
    
    StringToNumber = (input) => input.length;
    
    StringToNumber = (input) => input.toUpperCase();
    // Type 'string' is not assignable to type 'number'.
    ```


### 그 외 반환 타입

- `void` 반환 타입
  - 반환 타입이 `void`인 함수는 값 반환을 허용하지 않습니다.

      ```tsx
      function logSing(song: string | undefined): void {
          if(!song) {
              return;
          }
      
          return true; // Type 'boolean' is not assignable to type 'void'.
      }
      ```

  - **자바스크립트의 함수는 실젯값이 반환되지 않으면 `undefined`를 반환**하지만, `void`는 함수의 반환 타입이 무시된다는 것을 의미하고 `undefined`는 반환되는 리터럴 값입니다. `undefined`와 `void`를 구분해서 사용하면 매우 유용합니다.

      ```tsx
      function returnVoid() {
          return;
      }
      
      let laztValue: string | undefined;
      
      laztValue = returnVoid();
      // Type 'void' is not assignable to type 'string | undefined'.
      ```

- `never` 반환 타입 - `never` 반환 함수는 (의도적으로) 항상 오류를 발생시키거나 무한 루프를 실행하는 함수입니다. `never` 키워드와 `never` 타입은 프로그래밍 언어에서 bottom 타입 또는 empty 타입을 뜻합니다. bottom 타입은 값을 가질 수 없고 참조할 수 없는 타입이므로 bottom 타입에 그 어떠한 타입도 제공할 수 없습니다. `never`와 `void`와는 다릅니다. `void`는 아무 것도 반환하지 않는 함수를 위한 것이고, `never`는 절대 반환하지 않는 함수를 위한 것입니다.

## 참고자료

- 러닝 타입스크립트(조시 골드버그, 2023)