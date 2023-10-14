### **문제 설명**

array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.

divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

### 제한사항

- arr은 자연수를 담은 배열입니다.
- 정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.
- divisor는 자연수입니다.
- array는 길이 1 이상인 배열입니다.

### 입출력 예

| arr           | divisor | return        |
| ------------- | ------- | ------------- |
| [5, 9, 7, 10] | 5       | [5, 10]       |
| [2, 36, 1, 3] | 1       | [1, 2, 3, 36] |
| [3,2,6]       | 10      | [-1]          |

### 입출력 예 설명

입출력 예#1

arr의 원소 중 5로 나누어 떨어지는 원소는 5와 10입니다. 따라서 [5, 10]을 리턴합니다.

입출력 예#2

arr의 모든 원소는 1으로 나누어 떨어집니다. 원소를 오름차순으로 정렬해 [1, 2, 3, 36]을 리턴합니다.

입출력 예#3

3, 2, 6은 10으로 나누어 떨어지지 않습니다. 나누어 떨어지는 원소가 없으므로 [-1]을 리턴합니다.

---

### 내 풀이

```jsx
function solution(arr, divisor) {
  let answer = [];
  for (let i = 0; i < arr.length; i++) {
    let reminder = arr[i] % divisor;
    if (reminder === 0) {
      answer.push(arr[i]);
    }
    // answer.sort((a, b) => a - b);
  }
  return answer.length === 0 ? [-1] : answer.sort((a, b) => a - b);
}
```

### 풀이 과정

- 반복문을 돌려서 배열의 각각의 값을 divisor로 나눴을 때 나머지값을 계산한다.
- 해당 나머지값이 0인 경우에만 배열에 담아주고, 값이 담기지 않은 배열은 [-1]로 return

### 남 풀이

```jsx
function solution(arr, divisor) {
  var answer = arr.filter((v) => v % divisor == 0);
  return answer.length == 0 ? [-1] : answer.sort((a, b) => a - b);
}
```

### 풀이 과정

- `Array.prototype.filter()`를 통해 값을 filter할 수 있음

### 참고 자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
