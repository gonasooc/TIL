### **문제 설명**

문자열 `my_string`과 정수 `k`가 주어질 때, `my_string`을 `k`번 반복한 문자열을 return 하는 solution 함수를 작성해 주세요.

---

### 제한사항

- 1 ≤ `my_string`의 길이 ≤ 100
- `my_string`은 영소문자로만 이루어져 있습니다.
- 1 ≤ `k` ≤ 100

---

### 입출력 예

| my_string | k   | result                                     |
| --------- | --- | ------------------------------------------ |
| "string"  | 3   | "stringstringstring"                       |
| "love"    | 10  | "lovelovelovelovelovelovelovelovelovelove" |

---

### 입출력 예 설명

입출력 예 #1

- 예제 1번의 `my_string`은 "string"이고 이를 3번 반복한 문자열은 "stringstringstring"이므로 이를 return 합니다.

입출력 예 #2

- 예제 2번의 `my_string`은 "love"이고 이를 10번 반복한 문자열은 "lovelovelovelovelovelovelovelovelovelove"이므로 이를 return 합니다.

---

### 내 풀이

```jsx
function solution(my_string, k) {
  let answer = "";
  for (let i = 0; i < k; i++) {
    answer = answer.concat(my_string);
  }
  return answer;
}
```

### 풀이 과정

- 반복문을 통한 `String.prototype.concat()` 으로 처리
  - `array`에서만 주로 사용해서 그런지 `string`에서는 적용이 안 되는 것으로 착각을 하고 있었음, 둘 다 적용 가능

### 남 풀이

```jsx
function solution(my_string, k) {
  return my_string.repeat(k);
}
```

### 풀이 과정

- `String.prototype.repeat()`가 있다는 건 처음 알았다. 해당 문자열을 전달 인자(count) 만큼 반복
