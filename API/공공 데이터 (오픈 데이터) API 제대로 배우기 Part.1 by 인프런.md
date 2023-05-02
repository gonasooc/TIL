**공공데이터 API 이용하기 - 전체적인 흐름 미리보기**

[공공데이터 포털](https://www.data.go.kr/)

[Kakao Developers](https://developers.kakao.com/)

- 발급 후 인증키는 Encoding/Decoding 두 개 발급

**API 개념이해**

- Application Programming Interface

**JSON 개념이해**

- Javascript Object Notation → JavaScript에서 객체를 만들 때 사용하는 표현식
- 프로그래밍 언어나 문법은 아니고 단지 하나의 데이터 저장 방식
- **객체 상태로 데이터를 전달할 수는 없기에 객체(Object)를 문자열(String)로 변환해서 전달해줘야 함**
- 받은 쪽에서는 다시 문자열을 객체로 변환해야지만 해당 프로그래밍 언어에서 객체로서 사용할 수 있음
- 이러한 변환 과정을 잘 숙지하고 파싱하여 사용하는 것이 필요
- 프로퍼티나 값을 쌍따옴표로 처리
- 예전에 주로 사용하던 XML을 대체한 경향이 있음 → XML은 비교적 헤비하고 복잡함

**JSON 데이터를 다루기 위한 JS 기본 사용법1**

```jsx
const person = [];
console.log(...person); // 전개 연산자, 배열이 가능한 경우 전개
const str1 = 'korea';
console.log(...str1); // k o r e a - node 객체 또한 전개 가능
console.log([...str1]); // ['k', 'o', 'r', 'e', 'a'] - 배열에 담아서 전개
console.log({...str1}); // {0: 'k', 1: 'o', 2: 'r', 3: 'e', 4: 'a'} - 객체에 담아서 전개
```

**JSON 데이터를 다루기 위한 JS 기본 사용법2**

for in, for of

**JSON 데이터 객체와 문자열로 변환하기**

JSON.parse(), JSON.stringify()