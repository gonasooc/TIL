### **문제 설명**

문자들이 담겨있는 배열 `arr`가 주어집니다. `arr`의 원소들을 순서대로 이어 붙인 문자열을 return 하는 solution함수를 작성해 주세요.

---

### 제한사항

- 1 ≤ `arr`의 길이 ≤ 200
  - `arr`의 원소는 전부 알파벳 소문자로 이루어진 길이가 1인 문자열입니다.

---

### 입출력 예

| arr           | result |
| ------------- | ------ |
| ["a","b","c"] | "abc"  |

---

### 내 풀이

```jsx
function solution(arr) {
  var answer = arr.join("");
  return answer;
}
```

### 풀이 과정

- 배열을 이어진 문자열로 변환 → `Array.prototype.join()`
