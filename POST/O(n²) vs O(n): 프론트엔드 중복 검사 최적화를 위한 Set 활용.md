### 배열 순회 vs `Set`

프론트 개발을 하다 보면 사용자가 입력한 값이나 서버에서 받은 데이터 중에서 중복된 항목이 존재하는지 확인해야 하는 경우가 더러 있다. 나의 경우엔 음악 메타데이터를 입력하거나 업로드할 때, `UCI`, `ISRC` 같은 고유 식별자는 중복이 있으면 안 되기 때문에 프론트에서 1차 검증 로직이 필요했다.

검증해야 할 값이 많지 않다면 단순히 배열을 순회하면서 검증하면 될 일이지만 값이 많다면, 혹은 값이 많아질 가능성이 있다면 효율적이지 않을 수 있기에 `Set`을 주로 활용하기 마련이다. `Set`을 사용하는 게 어떤 면에서 효율적이기에 `Set`을 사용하는지에 의문이 생긴다. 굳이 `Set`을 써야 할까?

최근 프로젝트에서 이와 관련된 작업을 하다가 실제로 `Set`을 활용했는데, 그 경험을 바탕으로 배열을 순회하는 것과 `Set`을 사용하는 것의 성능, 가독성, 실용성 측면에서의 차이를 간략하게 정리해 본다.

### 배열 순회를 이용한 중복 검사

가장 직관적인 방식은 배열을 직접 순회하면서 이전에 나왔던 값과 비교하는 방법이다.

```jsx
const hasDuplicate = (arr) => {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) return true;
    }
  }
  return false;
};
```

혹은 조금 더 간결하게 적는다면,

```jsx
const hasDuplicate = arr => arr.some((val, index) => arr.indexOf(val) !== index);
```

두 방식 모두 배열 요소를 하나씩 비교하므로 반복 횟수가 많아질 수 있다. 첫 번째 방식은 명시적인 이중 for문을 사용하고, 두 번째 방식은 `some`과 `indexOf`를 조합하지만 내부적으로는 유사한 작업을 수행한다고 볼 수 있다. 만약 값이 수백 개나 수천 개가 된다면? 배열 순회의 성능 이슈는 여기서 나올 수 있다.

### `Set`을 활용한 중복 검사

`Set`은 ECMAScript 2015(ES6)부터 도입된 중복되지 않는 값의 집합을 저장하는 객체다. MDN는 [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)에 대해 다음과 같이 설명하고 있다.

> “Set objects are collections of values. A value in the Set may only occur once.”
> 

Set 안에서 값은 한 번만 발생할 수 있다, 즉 중복되지 않은 데이터의 집합이라고 볼 수 있다. 이러한 특성 덕분에 중복 여부를 확인할 때 아래와 같은 방식으로 간단하게 사용하는 것이 가능하다.

```jsx
const hasDuplicate = (arr) => {
  const seen = new Set();
  for (const val of arr) {
    if (seen.has(val)) return true;
    seen.add(val);
  }
  return false;
};
```

또는 좀 더 간결하게,

```jsx
const hasDuplicate = (arr) => new Set(arr).size !== arr.length;
```

### `Set`의 내부 구현

ECMAScript 2015는 `Set`뿐만 아니라 `Map`, `WeakSet`, `WeakMap` 은 내부적으로 해시 테이블(Hash Table) 구조를 사용한다. 해시 테이블은 키를 해시 함수에 통과시켜 나온 값을 인덱스로 활용하는 자료구조다. 이 특성 덕분에 데이터 접근, 삽입, 삭제 작업이 평균적으로 O(1)의 시간 복잡도를 가진다.

V8 엔진의 `Set` 구현을 살펴보면, 내부적으로 해시 함수는 주어진 키를 해시 테이블의 특정 위치에 매핑하는 데 사용한다. ECMAScript 명세에 정확히 명시되어 있지는 않지만, 대부분의 JavaScript 엔진이 유사한 방식으로 구현한다고 한다.

### `Set`의 시간복잡도는?

| **연산** | **배열 순회** | **Set** |
| --- | --- | --- |
| **중복 검사** | O(n²) | O(n) |
| **요소 추가** | O(1) | O(1) |
| **요소 접근** | O(1) | - |
| **특정 값 검색** | O(n) | O(1) |
| **요소 삭제** | O(n) | O(1) |

`Set`의 `has()`와 `add()` 메서드는 내부적으로 매우 빠르게 동작하는데, 배열 순회와의 가장 큰 차이라면 시간복잡도가 평균적으로 O(1), 즉 데이터 양과 상관없이 거의 일정한 속도로 처리된다. 즉, 배열을 처음부터 끝까지 한 번만 순회하면서 중복 여부도 함께 검사할 수 있는 구조이기 때문에 데이터가 많아질수록 훨씬 효율적이다.

시간복잡도(Time Complexity)는 알고리즘이 처리해야 할 데이터의 양이 늘어날수록, 실행 시간이 얼마나 증가하는지를 수치로 표현한 것이다.

예를 들어 위처럼 배열을 중첩해서 순회하는 방식은 데이터가 `n`개일 때 최대 `n * n`번 비교하게 되므로 O(n²)라고 표현하게 된다. 이는 10개의 데이터일 때는 100번, 100개의 데이터일 땐 10,000번 비교해야 하기에, 작은 데이터일 때는 큰 문제가 없지만 수백~수천 개의 데이터가 들어오면 성능 이슈가 생길 수 있어 이는 사용성 저하로 이어질 가능성이 높다.

예컨대 앞서 배열에서 중복을 검사하는 `indexOf`나 `includes` 메서드는 모든 요소를 순회해야 하므로 O(n)의 시간복잡도를 가진다. 이를 모든 요소에 대해 수행하면 전체 시간복잡도는 O(n²)가 된다. 반면 `Set`은 `has()` 메서드가 O(1)의 시간복잡도를 가지므로 모든 요소를 한 번씩만 순회하면 되어 전체 시간복잡도는 O(n)이 된다.

### 트레이드 오프: 메모리 사용량 비교

`Set`은 해시 테이블 구현으로 인해 일반적으로 배열보다 더 많은 메모리를 사용한다. 아래와 같이 브라우저 내에서 [performance 객체의 memory property](https://developer.mozilla.org/en-US/docs/Web/API/Performance/memory)를 통해서 간단한 확인이 가능하지만, 비표준이고 deprecated 된 property라는 점을 참고해야 한다. 정확한 비교를 위해서는 Node.js 환경에서 가비지 컬렉션을 강제하여 측정하거나, 메모리 탭에서 Heap snapshot을 찍는 것이 좋다. 

```jsx
const size = 5000000; // 500만 개 데이터

// Array 메모리 측정
const beforeArray = performance.memory ? performance.memory.usedJSHeapSize : 0;
const arr = Array.from({ length: size }, (_, i) => i);
const afterArray = performance.memory ? performance.memory.usedJSHeapSize : 0;
console.log(`Array 메모리: ${((afterArray - beforeArray) / 1024 / 1024).toFixed(2)} MB`);

// Set 메모리 측정
const beforeSet = performance.memory ? performance.memory.usedJSHeapSize : 0;
const set = new Set(Array.from({ length: size }, (_, i) => i));
const afterSet = performance.memory ? performance.memory.usedJSHeapSize : 0;
console.log(`Set 메모리: ${((afterSet - beforeSet) / 1024 / 1024).toFixed(2)} MB`);
```

`arr`과 `set` 변수가 생성되기 전과 후의 메모리 비교를 통해 어느 정도 할당되었는지 확인할 수 있다. 환경에 따라 차이가 있을 수 있겠지만, 아래와 같은 결괏값이 나온다.

```jsx
Array 메모리: 19.08 MB
Set 메모리: 178.28 MB
```

`Set`이 배열보다 확실히 더 많은 메모리를 사용하지만, 대량의 데이터셋이라고 가정하면 성능 개선이라는 이점에 비해 감수할 만한 트레이드 오프이지 않나 싶다.

### 브라우저 호환성과 실행 환경 차이

`Set`은 앞서 말한 것처럼 ES6에서 새로 도입되었기 때문에 IE 같은 구형 브라우저에서는 버전에 따라 별도의 폴리필이 필요할 순 있다. 하지만 [caniuse.com](http://caniuse.com) 에서 검색해 보면 Global 기준 96% 이상이 Set을 지원하는 브라우저를 사용하고 있기 때문에 필요하다면 사용하는 데는 큰 무리가 없다. 브라우저 엔진에 따라 일부 성능 차이가 존재할 수 있다.

### 실무에서 `Set`을 사용하는 이유

내가 작업했던 코드는 다음과 같다.

```tsx
const validateTrackInfo = () => {
  const uciSet = new Set();
  const isrcSet = new Set();
  const trackCodeSet = new Set();

  for (const key in trackInfo) {
    const track = (trackInfo as { [key: string]: any })[key];
    if (uciSet.has(track.uci)) {
      return false;
    }
    if (isrcSet.has(track.isrc)) {
      return false;
    }
    if (trackCodeSet.has(track.trackCode)) {
      return false;
    }
    if (track.uci) uciSet.add(track.uci);
    if (track.isrc) isrcSet.add(track.isrc);
    if (track.trackCode) trackCodeSet.add(track.trackCode);
  }
  return true;
};
```

이 코드에서 `Set`을 사용한 이유는 다음과 같다.

- 음반은 박스셋의 존재와 더불어 트랙 데이터가 수십~수백 개로 많아질 가능성이 있기 때문에 반복 순회를 줄이고 빠르게 중복 여부를 판단할 필요가 있다.
- 각 트랙의 `UCI`, `ISRC`, `trackCode`는 고유 식별자이므로 하나라도 중복됐을 때 에러 처리가 필요하고, Set을 사용함으로써 ‘중복을 허용하지 않는 고윳값의 집합’이라는 의도를 명확히 표현할 수 있다.
- `Set`의 `has()`는 해당 값이 이미 존재하는지를 빠르게 판단해 주므로 빠른 탐색이 가능하다.
- 각 중복 항목에 대해 각각 별도의 `Set`을 두어 상대적으로 가독성이 좋은 구조를 만들 수 있다.

### `Set`을 활용한 중복 제거

일반적으로 `Set`은 중복 확인보다 중복 요소를 쉽게 제거할 수 있어 아래와 같이 활용하는 경우가 많았다.

```jsx
const arr = [1, 2, 2, 3, 4, 4, 5];
const uniqueArr = [...new Set(arr)];  // [1, 2, 3, 4, 5]
```

이 방식은 `filter()`나 `reduce()`를 활용한 로직보다 상대적으로 간단하고, 무엇보다 실행 속도도 빠르다.

### 어떤 상황에서 어떤 방법을 쓸까?

| **구분** | **배열 순회** | **`Set`** |
| --- | --- | --- |
| **시간복잡도** | O(n²) (느림) | O(n) (빠름) |
| **코드 가독성** | 비교적 복잡 | 간결하고 직관적 |
| **활용도** | 중복 확인에 한정 | 중복 확인 및 제거 등 유연한 사용 |
| **추천 상황** | 적은 데이터, 복잡한 조건 비교 | 대량 데이터, 단순 중복 체크 및 제거 |

### 마무리하며

개발을 하다 보면, 단순히 돌아가는 코드만 짜는 게 아니라 '가장 효율적이고 유지 보수하기 좋은 방법'을 선택하는 것이 중요해진다. `Set`은 중복을 허용하지 않는다는 명확한 특성 덕분에 성능, 가독성, 실용성 측면에서 많은 장점을 제공한다. 특히 중복 데이터를 자주 다루는 경우라면 `Set`을 익숙하게 써두는 것이 장기적으로 훨씬 도움이 될 수 있다.

이번 실무 경험을 통해 실제로 배열 순회와의 차이점을 이해하고 다시 한번 `Set`의 유용함을 체감할 수 있었다. 다만, 작은 데이터셋이나 메모리가 제한된 환경에서는 배열 순회가 더 효율적일 수 있다는 점은 유의할 필요가 있다. 늘 그렇듯, 적절한 도구를 적절한 상황에 사용하는 것이 최선의 방법이다.

### 참고 자료

- https://www.daleseo.com/js-set/
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set
- https://v8.dev/blog/hash-code
- [https://positiveko-til.vercel.app/til/cs/hashtable.html#자바스크립트로-해시-테이블-구현하기](https://positiveko-til.vercel.app/til/cs/hashtable.html#%E1%84%8C%E1%85%A1%E1%84%87%E1%85%A1%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%E1%84%85%E1%85%A9-%E1%84%92%E1%85%A2%E1%84%89%E1%85%B5-%E1%84%90%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%87%E1%85%B3%E1%86%AF-%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5)
- [https://hsb422.tistory.com/entry/ㅁ-자바-Set에-데이터-추가-삭제가-O1인-이유](https://hsb422.tistory.com/entry/%E3%85%81-%EC%9E%90%EB%B0%94-Set%EC%97%90-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B6%94%EA%B0%80-%EC%82%AD%EC%A0%9C%EA%B0%80-O1%EC%9D%B8-%EC%9D%B4%EC%9C%A0)
- https://developer.mozilla.org/en-US/docs/Web/API/Performance/memory
- https://caniuse.com/mdn-javascript_builtins_set