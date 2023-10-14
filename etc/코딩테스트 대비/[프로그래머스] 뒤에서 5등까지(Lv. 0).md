### **문제 설명**

정수로 이루어진 리스트 `num_list`가 주어집니다. `num_list`에서 가장 작은 5개의 수를 오름차순으로 담은 리스트를 return하도록 solution 함수를 완성해주세요.

---

### 제한사항

- 6 ≤ `num_list`의 길이 ≤ 30
- 1 ≤ `num_list`의 원소 ≤ 100

---

### 입출력 예

| num_list                   | result             |
| -------------------------- | ------------------ |
| [12, 4, 15, 46, 38, 1, 14] | [1, 4, 12, 14, 15] |

---

### 입출력 예 설명

입출력 예 #1

- [12, 4, 15, 46, 38, 1, 14]를 정렬하면 [1, 4, 12, 14, 15, 38, 46]이 되고, 앞에서 부터 5개를 고르면 [1, 4, 12, 14, 15]가 됩니다.

---

### 내 풀이

```jsx
function solution(num_list) {
  var answer = num_list.sort((a, b) => a - b).slice(0, 5);
  return answer;
}
```

### 풀이 과정

- 일단 배열의 오름차순 필요, `arr.sort()` 사용
  - `arr.sort()` 사용 시 배열의 요소는 문자열로 취급 받기 때문에 [1,12,14,15,38,4,46]가 나옴, 별도의 새로운 함수를 넘겨줘야 함
- `arr.slice()`를 통해서 5개의 수를 반환
  - `arr.slice()`는 `end` 인덱스를 제외하고 추출한다는 사실 리마인드

### 참고자료

- https://ko.javascript.info/array-methods#ref-341
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice
