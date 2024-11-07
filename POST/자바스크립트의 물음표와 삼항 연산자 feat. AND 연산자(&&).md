### ReferenceError: "x" is not defined || TypeError: Cannot read properties of undefined (reading 'x')

아마도 개발자로 커리어를 시작하면서 가장 많이 마주한 에러라면 이 두 가지인 것 같다. 애초에 JavaScript는 동적 언어면서 동시에 런타임 에러를 발생하기도 하고, 특히 초기에는 예외 처리에 대한 노하우가 부족해서 종종 콘솔창에서 에러를 마주했던 기억이 난다. 그 무렵에는 Vue를 사용하고 있었고, 중첩 객체에서 `null`이나 `undefined`를 통해 해당 에러를 마주하면 상위 요소에 `v-if`를 통해 DOM 요소를 그리지 않는 방향으로 해결하기도 했었다.

### 자바스크립트의 다양한 ‘?’

TypeScript의 정적 타이핑을 통해 컴파일 과정에서 에러를 마주하는 게 아니라면 결국 JavaScript 환경에서는 예외 처리를 통해 에러를 방지해왔다. 중첩 객체에서 `?.`를 통해 `falsy`를 체크하는 옵셔널 체이닝 연산자, `??`를 통해 `null`과 `undefined`를 체크하는 null 병합 연산자, 조건을 통해 예외 처리를 할 수 있는 삼항 연산자가 있다. 이들은 비슷하면서도 묘하게 사용처가 다르다.

### 여기에 이걸 왜 썼지?

```jsx
export function APIUrl(url: String): string {
	return (process.env.NEXT_PUBLIC_API_URL ?? "") + url
}
```

다른 사람의 코드를 둘러보다가 null 병합 연산자를 사용한 부분을 보고 왜 여기에 이걸 썼을까 하는 의문이 들어 이번 기회에 그 ‘비슷하면서도 다름’을 기본부터 다시 짚어보고자 했다.

### null 병합 연산자(Nullish coalescing operator)

null 병합 연산자(`??`)는 왼쪽 피연산자가 `null`이나 `undefined`일 때 오른쪽 피연산자를 반환하고, 아니라면 왼쪽 피연산자를 반환하는 논리 연산자이다. 비슷한 논리 연산자 OR(`||`)가 있지만, 이 경우 `null`이나 `undefined` 외에 ‘’ 또는 `0`과 같은 `falsy` 값까지도 오른쪽 피연산자를 반환하게 되니 구분 지을 필요가 있다. 

null 병합 연산자는 명확하게 `null`이나 `undefined`인 경우에 기본값을 할당하고자 할 때 사용하는 게 좋은 것 같다. 일반적으로 `||` 를 통해 기본값을 할당하는 경우도 있는데, 이 경우 앞서 말한 것처럼 `null`이나 `undefined` 외의 `falsy`한 값(`0`, `‘’`, `NaN`) 또한 반환되지 않는다. 실제로 화면 단위에서 `0`, `‘’`, `NaN` 이런 값들을 사용하거나 표기해 줘야 하는 경우 null 병합 연산자를 사용해 기본값을 할당하면 목적에 맞게 표기하는 것이 가능하다.

```jsx
let myText = ""; // An empty string (which is also a falsy value)

let notFalsyText = myText || "Hello world";
console.log(notFalsyText); // Hello world

let preservingFalsy = myText ?? "Hi neighborhood";
console.log(preservingFalsy); // '' (as myText is neither undefined nor null)
```

같이 써본 적이 없어서 잘 몰랐는데, MDN 문서를 보니 null 병합 연산자와 AND(`&&`), OR(`||`) 연산자와 이어서 바로 사용할 수 없다. 안정성 관련 이슈 때문이라고 하는데, 괄호를 통해 우선 순위를 명시적으로 나타내면 사용 가능하다. 

```jsx
(null || undefined) ?? "foo"; // returns "foo"
```

### 옵셔널 체이닝(Optional chaining)

아마도 가장 많이 사용한 문법이지 않을까 싶다. 기본적으로 프론트에서 API 연동을 하다 보면 객체를 다루게 되고, 중첩 객체의 값이 유효하지 않다면 ReferenceError를 마주할 수 있기 때문에 가능하면 옵셔널 체이닝을 통해 안전한 예외 처리를 하는 편이다. 옵셔널 체이닝은 왼쪽 연산자가 `null`이나 `undefined`인 경우 `undefined`를 반환하고, 그렇지 않은 경우 오른쪽 `property`의 참조를 가져간다.

일반적으로 아래와 같이 중첩 객체의 하위 속성을 찾을 때 주로 사용하였다. 물론 삼항 연산자를 사용할 일은 거의 없지만 사용한다면 저런 식으로 하위 속성의 유무를 체크할 수 있다.

```jsx
let nestedProp = obj.first && obj.first.second;
```

```jsx
let nestedProp = obj.first.second ? obj.first.second : null;
```

옵셔널 체이닝의 등장 이후엔 이렇게 간소화하는 것이 가능하다.

```jsx
let nestedProp = obj.first?.second;
```

이 또한 실무에서 사용한 적은 없는데 함수 호출에서도 옵셔널 체이닝을 사용할 수 있다. 존재하지 않는 메소드를 호출했을 때 옵셔널 체이닝을 사용했다면 예외 발생 대신 자동으로 `undefined`를 반환한다.

```jsx
let result = someInterface.customMethod?.();
```

### 조건(삼항) 연산자(Conditional (ternary) operator)

말 그대로 3개의 피연산자를 받는 유일한 연산자이다. 가장 앞에는 조건문, `?`를 거쳐 중간에는 `truthy` 값이 올 경우 실행할 표현식, `:`을 거쳐 `falsy` 값이 올 경우 실행할 표현식 순서로 작성한다. JSX 내에서 조건문 대신 사용하고, Vue에서도 일부 로직 안에서 썼었기 때문에 익숙하다. 심플하고 가독성이 좋아서 주로 사용하게 되지만, 삼항 연산자를 여러 번 중첩하게 되면 가독성이 떨어지기 때문에 누군가의 핀잔을 받을 가능성이 높다. 코드 작성 성향에 따라 다를 수 있지만 개인적으로 삼항을 중첩해서 사용하느니 별도의 조건문 함수로 빼서 작성하거나 함수 내에서 early return을 통해 작성하는 걸 선호하는 편이다.

### feat. AND 연산자(`&&`)

`&&`의 경우 상대적으로 구문이 짧고 가독성이 좋아서 선호되기도 한다. 이 또한 취향 차이라고 생각하지만, 삼항 연산자를 통해 좀 더 안전하게 사용하는 편을 선호한다. [React 공식 문서](https://ko.react.dev/learn/conditional-rendering#logical-and-operator-)에서도 언급하고 있지만 `&&`의 경우 왼쪽 피연산자에 `0`이 오는 경우 `0`을 렌더링한다. `useState`를 통해 `messageCount`를 선언하고 특정 이벤트를 통해 값이 변하는 경우 `0`이 나올 수 있는데, 아래와 같이 선언한다면 `0`이 렌더링될 수도 있다.

```jsx
messageCount && <p>New messages</p>
```

물론 좌측 피연산자 쪽에 특정 조건을 넣어 `&&`를 사용하는 것이 가능하다.

```jsx
messageCount > 0 && <p>New messages</p>
```

다만 휴먼 에러는 언제든 존재할 수 있기 때문에 삼항 연산자를 통해 미연에 방지하는 게 안전하다고 생각하는 편이다.

```jsx
messageCount ? messageCount : <p>New messages</p>
```

### 그래서 왜 썼냐고

```jsx
export function APIUrl(url: String): string {
	return (process.env.NEXT_PUBLIC_API_URL ?? "") + url
}
```

사실 완전히 리팩토링이 끝난 코드가 아니라 리팩토링 중인 코드를 살펴본 거라 예외 처리가 필요하다는 점을 명시적으로 표기한 게 아닐까 싶다. 물론 이런 예외 처리보다 `NEXT_PUBLIC`이라는 prefix로 시작하는 경우 클라이언트에서 접근하는 환경변수이기 때문에 Zod 같은 라이브러리를 통해 해당 변수의 유효성 검증이 가능하다. 앞서 null 병합 연산자의 예시처럼 `switch`문에서의 `default`처럼 명확하게 정의할 기본값이 필요할 때 사용하는 편이 더 나은 것 같기도 하다. 어쨌든 덕분에 JavaScript를 공부했으니 럭키비키잖아?

### 참고자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing
- https://ko.javascript.info/nullish-coalescing-operator
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Optional_chaining
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Conditional_operator
- https://ko.react.dev/learn/conditional-rendering#logical-and-operator-
- https://i-ten.tistory.com/232
- https://www.hojinlee.dev/posts/zod-environment-variables-validation/