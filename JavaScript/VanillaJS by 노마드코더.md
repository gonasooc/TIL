[Three.js - JavaScript 3D library](https://threejs.org/)

[Electron | Build cross-platform desktop apps with JavaScript, HTML, and CSS.](https://www.electronjs.org/)

[Socket.IO](https://socket.io/)

### **cli로 폴더 생성 및 vsc 실행**

```jsx
mkdir momentum // momentum 폴더 생성
code momentum // 해당 폴더를 vsc로 실행
```

### **const and let**

- let은 값을 변경, const는 값을 변경할 수 없음
- 다른 사람의 코드를 보더라도 let을 선언했는지, const를 선언했는지에 따라 코딩의 의도를 파악할 수도 있음
- 과거에는 var만 존재했는데 var는 let처럼 값을 변경할 수 있지만 반대로 데이터가 보호 받을 수 없음
- var를 써도 되지만 이왕이면 let과 const를 사용하는 게 좋음

### **Booleans**

- 예컨대 사용자가 로그인 했는가, 안 했는가 이런 게 필요할 땐 다른 별도의 데이터가 필요 없고 true, false만 있으면 되기에 그럴 때 사용
- null은 그 변수에 아무 것도 없음을 뜻함 → null은 false와는 다름 → false는 false라는 값이 존재하고 null은 null이라는 타입이 존재하는 것
- undefined는 해당 변수에 값 자체를 부여하지 않은 상태 → 공간은 있는데 값이 들어가지 않은 것
- 이해하기 어렵지만 반드시 기억해야 할 것은 null은 절대 자연적으로 발생하지 않는다는 것

### **Array**

- arrayname.push() → 해당 배열값 끝에 데이터 추가

```jsx
const daysOfWeek = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat'];

// Get Item from Array
console.log(daysOfWeek);

// Add one more day to the array
daysOfWeek.push('sun');
console.log(daysOfWeek);
```

### **Objects**

일반 변수 선언보다 object 형태의 선언이 한 개의 개체(entity), 즉 player에 대해서 설명하고 있다는 걸 알기 때문에 더 나음

```jsx
// 일반 변수 선언
const playerName = 'nico';
const playerPoints = 121212;
const playerHandsome = false;
const playerFat = 'little bit';

// 객체 형태의 변수 선언
player.name
player.points
player.handsome
player.fat
```

배열로 선언하게 되면 각각의 value가 어떤 의미인지 알 수가 없음

```jsx
// 일반 변수 선언
const playerName = 'nico';
const playerPoints = 121212;
const playerHandsome = false;
const playerFat = 'little bit';

// 배열 형식의 선언
const player = ['nico', '121212', false, 'little bit'];
```

그래서 더 나은 방식이 필요한 거임, 그것이 object

→ object 안에서는 =을 사용하지 않음, property와 value만 :로 연결

```jsx
// 일반 변수 선언
const playerName = 'nico';
const playerPoints = 121212;
const playerHandsome = false;
const playerFat = 'little bit';

// object 형식의 선언
const player = {
  name: "nico",
  points: "121212",
  handsome: false,
  fat: 'little bit'
};
```

언제든지 property는 추가할 수 있음

```jsx
// object 형식의 선언
const player = {
  name: "nico",
  points: "121212",
  handsome: false,
  fat: 'little bit'
};
console.log(player);
**player.lastName = "potato";**
console.log(player);
```

### **Functions**

함수를 선언하고 parameter를 사용하는 방식

```jsx
function sayHello(nameOfPerson, age) {
  console.log('Hello! my name ' + nameOfPerson + ' and I\'m ' + age)
}

sayHello('Nas', 10)
```

### **return**

function 안에서 사용자가 데이터를 얻고자 할 때

```jsx
const age = 96;
function calcKrAge(worldAge) {
  return worldAge + 2; // 98을 return
	// 만약 return이 없다면 undifined를 반환
}

const krAge = calcKrAge(age);

console.log(krAge);
```

```jsx
const calculator = {
  plus: function(a, b) {
    return a + b;
  },
  minus: function(a, b) {
    return a - b;
  },
  times: function(a, b) {
    return a * b;
  },
  divide: function(a, b) {
    return a / b;
  },
  power: function(a, b) {
    return a ** b;
  }
};

const plusResult = calculator.plus(2, 3);
const minusResult = calculator.minus(plusResult, 3);
const timesResult = calculator.times(2, minusResult);
const divideResult = calculator.divide(plusResult, timesResult);
const powerResult = calculator.divide(divideResult, plusResult);
console.log(plusResult);
console.log(minusResult);
console.log(timesResult);
console.log(divideResult);
console.log(powerResult);
```

- 단 return이 실행되는 순간 function은 종료됨 → ex) return 밑에 console.log가 있다고 해도 console.log는 실행되지 않음

### **Conditionals**

- 숫자, 글자 구분하는 조건문 (isNaN 사용)

```jsx
const age = parseInt(prompt("How old are you?"));

if(isNaN(age)) {
  console.log("Please write a number.");
} else {
  console.log(age);
}
```

- else if를 통한 조건 추가

```jsx
const age = parseInt(prompt("How old are you?"));

if(isNaN(age)) {
  console.log("Please write a number.");
} else if(age < 18) {
  console.log("You are too young.")
} else {
  console.log("You can drink.")
}
```

- else는 선택사항임, 항상 써야 하는 건 아님

```jsx
const age = parseInt(prompt("How old are you?"));

if(isNaN(age) || age < 0) {
  console.log("Please write a real positive number.");
} else if(age < 18) {
  console.log("You are too young :(")
} else if(age >= 18 && age <= 50) {
  console.log("You can drink.")
} else if(age > 50 && age <= 80) {
  console.log("You'd better stop drinking :)")
}
```

- The Document Object

HTML안에 있는 내용을 document라는 ojbect로 담아줌

ex) document.title, document.body, document.location

```jsx

document.title = "Hello, JS!"
```

- 해당 element를 변수에 담으면 object로 담겨서 사용할 수 있게 됨 → console.dir로 출력해서 해당 object 구조 보는 게 도움됨

```jsx
let ttl = document.getElementById("ttl");
**console.dir(ttl);**

ttl.innerText = "Got you!"

console.log(ttl.className)
console.log(ttl.id)
```

- document.querySelector는 CSS selector를 통해 element에 접근 가능