### **문제 설명**

`i`팩토리얼 (i!)은 1부터 i까지 정수의 곱을 의미합니다. 예를들어 5! = 5 _ 4 _ 3 _ 2 _ 1 = 120 입니다. 정수 n이 주어질 때 다음 조건을 만족하는 가장 큰 정수 i를 return 하도록 solution 함수를 완성해주세요.

- i! ≤ n

---

### 제한사항

- 0 < `n` ≤ 3,628,800

---

### 입출력 예

| n       | result |
| ------- | ------ |
| 3628800 | 10     |
| 7       | 3      |

---

### 입출력 예 설명

입출력 예 #1

- 10! = 3,628,800입니다. `n`이 3628800이므로 최대 팩토리얼인 10을 return 합니다.

입출력 예 #2

- 3! = 6, 4! = 24입니다. `n`이 7이므로, 7 이하의 최대 팩토리얼인 3을 return 합니다.

---

### 내 풀이

```jsx
function solution(n) {
  let initNumber = 1;
  let factorialNumber = 1;
  for (let i = 1; i <= 10; i++) {
    initNumber = initNumber * i;
    if (initNumber > n) {
      break;
    }
    factorialNumber = i;
  }
  return factorialNumber;
}
```

### 풀이 과정

- 팩토리얼은 1까지 곱하기 때문에 `initNumber`를 1로 주고,
- 가장 작은 팩토리얼도 1이라(0!도 1) `factorialNumber`의 초기값도 1로 할당해줌
- 제한사항이 10이었기 때문에 반복 횟수로 설정하고, 인자로 넘겨준 n이 `initNumber`보다 작아지는 순간 `break;`
- 재귀함수로 접근해보려고 했는데 아직은 명확히 로직이 안 잡혀서 나중에 다시 해봐야겠다.
- 유튜브 팩토리얼 관련 강의 중에 퍼뮤테이션을 팩토리얼로 표현하는 부분이 재밌었다.

### 참고자료

- https://youtu.be/ByAh6UwtK1A?si=cXCEmw71XUarGDY0
- https://johnleeedu.tistory.com/23
