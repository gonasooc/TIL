### **문제 설명**

문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요.

s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.

### 제한 사항

- str은 길이 1 이상인 문자열입니다.

### 입출력 예

| s         | return    |
| --------- | --------- |
| "Zbcdefg" | "gfedcbZ" |

---

### 내 풀이

```jsx
function solution(s) {
  var answer = s.split("").sort().reverse().join("");
  return answer;
}
```

### 풀이 과정

- 정렬하기 위해 문자열을 배열로 변경
  - `String.prototype.split()`
- sort로 오름차순 정렬
  - 오름차순 정렬의 순서는 숫자 > 영어 대문자 > 영어 소문자 > 한글
- 배열의 순서 반전
  - `Array.prototype.reverse()`
- 배열의 모든 요소를 다시 하나의 문자열로 변경
  - `Array.prototype.join()`

### 참고 자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join
