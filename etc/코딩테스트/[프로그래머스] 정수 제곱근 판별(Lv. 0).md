### **문제 설명**

임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.

n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

### 제한 사항

- n은 1이상, 50000000000000 이하인 양의 정수입니다.

### 입출력 예

| n   | return |
| --- | ------ |
| 121 | 144    |
| 3   | -1     |

### 입출력 예 설명

**입출력 예#1**

121은 양의 정수 11의 제곱이므로, (11+1)를 제곱한 144를 리턴합니다.

**입출력 예#2**

3은 양의 정수의 제곱이 아니므로, -1을 리턴합니다.

---

### 내 풀이

```jsx
function solution(n) {
  let answer = 0;
  let x = Math.sqrt(n);
  if (Number.isInteger(x)) {
    let y = x + 1;
    answer = y * y;
  } else {
    answer = -1;
  }
  return answer;
}
```

### 풀이 과정

- n의 제곱근을 반환하는 `Math.sqrt()` 사용
- 해당 값이 정수인지 `Number.isInteger()` 체크

### 참고 자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger
