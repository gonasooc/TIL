# 객체와 클래스

## 객체 지향 프로그래밍이란?

- ‘객체’ 간의 상호작용을 중심으로 하는 프로그래밍
- 객체의 상태를 나타내는 ‘변수’ / 객체의 행동을 나타내는 ‘함수’
    - ex) 유저의 아이디나 유저의 생일 같은 상태의 변수 / 유저가 좋아요를 누르거나 유저가 상품을 구매하는 행동의 함수
        
        ```jsx
        const user = {
        	email: 'test@test.com',
        	birthdate: '1992-08-12',
        	like() {
        		// ...	
        	},
        	buy() {
        		// ...
        	}
        }
        ```
        
- **객체 지향 프로그래밍 → `property`와 `method`로 이루어진 각 객체들의 상호 작용을 중심으로 코드를 작성하는 것**
- 절차 지향 프로그래밍 → 변수와 함수를 가지고 작업의 순서에 맞게 코드를 작성하는 것

## **객체 만들기 1-1: Object-Literal**

- **중괄호를 쓰고 그 안에 `property`와 `method`를 나열하는 걸 Object-Literal '객체를 나타내는 문자열’**
- **this는 user 객체 자체를 이야기함**
    
    ```jsx
    const user = {
      email: 'test@test.com', // property
      birthdate: '1992-01-12',
      buy(item) {
        console.log(`${this.email} buys ${item.name}`); // method
        // **this는 user 객체 자체를 이야기함**
      }
    };
    // 중괄호를 쓰고 그 안에 property와 method를 나열하는 걸 Object-Literal '객체를 나타내는 문자열'
    
    const item = {
      name: '스웨터',
      price: 30000,
    };
    
    console.log(user.email)
    console.log(user.birthdate)
    user.buy(item);
    ```
    

## **객체 만들기 1-2: Factory function**

- Object-Literal로 user 하나만 추가해도 코드가 확 불어남
- **기존의 user 객체를 factory function 안쪽에 넣고 parameter로 값을 전달**
- **Object-Literal처럼 객체 하나하나 생성하는 게 아니라 createUser 함수를 정의하고 생성하는 걸 factory function**
- **property와 parameter의 이름이 같은 경우 값을 생략 가능**
    
    ```jsx
    function createUser(email, birthdate) {
      const user = {
        // email: email, // property: parameter
        email, // property와 parameter의 이름이 같은 경우 값을 생략해도 됨
        birthdate,
        buy(item) {
          console.log(`${this.email} buys ${item.name}`);
        },
      }
      return user;
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const user1 = createUser('test@test.com', '1992-02-23');
    const user2 = createUser('test2@test.com', '1962-01-12');
    const user3 = createUser('test3@test.com', '1942-01-12');
    // Object-Literal처럼 객체 하나하나 생성하는 게 아니라
    // 이렇게 생성하는 걸 factory function
    
    console.log(user1.email);
    console.log(user2.email);
    console.log(user3.email);
    
    user1.buy(item);
    user2.buy(item);
    user3.buy(item);
    ```
    

## **객체 만들기 2: Constructor function**

- 생성자 함수(Constructor Function)
- **this라는 키워드가 중요**
- **constructor function으로 객체를 생성하려면 new를 붙여야 함**
- **일반 함수와는 달리 첫번째 문자를 대문자로 적어줌**
    
    ```jsx
    function User(email, birthdate) {
      this.email = email;
      this.birthdate = birthdate;
      this.buy = function (item) {
        console.log(`${this.email} buys ${item.name}`);
      };
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const user1 = new User('test@test.com', '1992-01-01'); 
    const user2 = new User('test22@test.com', '1965-01-01');
    const user3 = new User('test33@test.com', '1965-01-01');
    
    console.log(user1.email);
    console.log(user2.email);
    console.log(user3.email);
    
    // - constructor function으로 객체를 생성하려면 new를 붙여야 함
    // - 일반 함수와는 달리 첫번째 문자를 대문자로 적어줌
    ```
    

## **객체 만들기 3: Class**

- 2015년 ES6에서 클래스(Class)가 추가됨
- JavaScript에서 조금 더 직관적이고 간편한 방법으로 객체지향 프로그래밍을 할 수 있게 해줌
- `**property`는 `constructor` 안에, `method`는 `constructor`바깥에 위치함**
    
    ```jsx
    // Constructor function
    // function User(email, birthdate) {
    //   this.email = email;
    //   this.birthdate = birthdate;
    //   this.buy = function (item) {
    //     console.log(`${this.email} buys ${item.name}`);
    //   };
    // }
    
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        this.email = email;
        this.birthdate = birthdate;
      }
    
      // 해당 객체의 메소드
      buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const user1 = new User('test@test.com', '1992-01-01'); 
    
    console.log(user1.email);
    console.log(user1.birthdate);
    user1.buy(item);
    
    const user2 = new User('test22@test.com', '1922-02-02'); 
    
    console.log(user2.email);
    console.log(user2.birthdate);
    user2.buy(item);
    ```
    

```
객체를 생성하는 방법
1. Object literal
2. Factory function
3. Constructor function
4. class
```

# 객체 지향 프로그래밍의 4개의 기둥

## **추상화**

- **어떤 구체적인 존재를 원하는 방향으로 간략화해서 나타내는 것**
- `class`, `property`, `method`의 이름을 잘 짓는 것이 중요
- 추상화된 요소를 별도로 설명하기 위해 문서를 작성하기도 함

## **캡슐화**

- **객체의 특정 `property`에 직접 접근하지 못하도록 막는 것**
- 예컨대 [`user1.email`](http://user1.email) 객체에 `name`에 해당하는 정보를 넣을 수도 있음
- 해당 개발자가 `user1.email`에 데이터를 담기 전에 `email` 형식인지 데이터를 검증하면 좋았겠지만, 거기에 의존할 수만은 없음
- **애초에 객체를 설계할 때 이런 부분을 방지하는 것이 중요함**
- **setter 메소드 → 앞에 set을 붙인 함수를 정의하게 되면 [user1.email](http://user1.email) = ‘chris robert’ 와 같이 직접 설정하려고 할 때마다 이 함수가 실행됨(값에 대한 유효성 검사)**
- **_email 같은 건 새롭게 설정된 property → 프로그래밍에선 숨기고자 하는 property의 이름을 맨 앞에 언더바의 형식으로 써줄 때가 많음**
- **getter 메소드 → property 값을 읽는 용도로 쓰기 때문에 parameter 필요 없음**
    
    ```jsx
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        this.email = email;
        this.birthdate = birthdate;
      }
    
      // 해당 객체의 메소드
      buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }
    
      get email() { // property 값을 읽는 용도로 쓰기 때문에 parameter 필요 없음
        return `Email Address is ${this._email}`;
      }
    
      // 앞으로 이 email이라는 property에 직접 값을 할당하려고 할 때마다
      // 해당 메소드가 실행됨 -> setter 메소드라고 부름
      // 값에 대한 유효성 검사
      **set email(address) {
        if (address.includes('@')) {
          this._email = address; // _email: 새롭게 설정된 property
        } else {
          throw new Error('invalid email address');
        }
      }**
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const user1 = new User('test@test.com', '1992-01-01'); 
    user1.email = 'chris_robert@gmail.com';
    console.log(user1.email);
    ```
    
- [ ]  **캡슐화 4,5 섹션 직접 체크해보기 + 클로저**

## **상속**

- **하나의 객체가 다른 객체의 `property`와 `method`를 물려받는 경우**
- 하단의 경우처럼 선언할 경우 → 코드가 반복된다는 부분 외에 한쪽은 수정하고 다른 한쪽은 수정하지 않는 휴먼에러가 생길 수 있음 → 이런 문제를 상속을 통해서 해결할 수 있음
- ex) 일반 유저/프리미엄 유저가 필요한 경우 → User의 `constructor`를 상속받되 level `property`가 추가로 필요한 경우
- 상속 적용 전(공통 요소가 중복 선언되어 있음) → 가독성 뿐만 아니라 유지보수 측면에서 좋지 않음
    
    ```jsx
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        **this.email = email;
        this.birthdate = birthdate;**
      }
    
      // 해당 객체의 메소드
      **buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }**
    }
    
    class PremiumUser {
      constructor(email, birthdate, level) {
        **this.email = email;
        this.birthdate = birthdate;**
        this.level = level;
      }
    
      **buy(item) {
        console.log(`${this.email} buys ${item.name}`)
      }**
    
      streamMusicForFree() {
        console.log(`Free music streaming for ${this.email}`);
      }
    }
    ```
    
- 상속 적용 → 코드의 재사용성이 좋아짐
- **extends User -> User에 선언된 property와 method를 그대로 물려받음**
- **derived class는 자식 클래스를 의미함 → 하기 코드는 상속 시 자식 클래스 안에서 super 생성자를 호출하지 않았기 때문에 에러(다음 챕터에서 설명)**
    
    ```jsx
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        this.email = email;
        this.birthdate = birthdate;
      }
    
      // 해당 객체의 메소드
      buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }
    }
    
    // extends User -> User에 선언된 property와 method를 그대로 물려받음
    // User: 부모 클래스, PremiumUser: 자식 클래스
    class PremiumUser extends User {
      constructor(email, birthdate, level) {
        this.level = level;
      }
    
      streamMusicForFree() {
        console.log(`Free music streaming for ${this.email}`);
      }
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const pUser1 = new PremiumUser('test_p1@gmail.com', '1995-02-23', 3);
    console.log(pUser1.email);
    console.log(pUser1.birthdate);
    console.log(pUser1.level);
    pUser1.buy(item);
    pUser1.streamMusicForFree();
    ```
    

## **super**

- `super`는 부모 클래스의 생성자 함수를 의미함
- **자식 클래스로 객체를 만드려고 할 때는 반드시 그 생성자 함수 안에서 super를 호출해서 부모 클래스의 생성자 함수를 먼저 호출해줘야 함(이것은 정해진 규칙)**
- `super`에는 부모 클래스의 생성자 함수와 동일한 값을 넘겨주면 됨
    
    ```jsx
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        this.email = email;
        this.birthdate = birthdate;
      }
    
      // 해당 객체의 메소드
      buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }
    }
    
    // extends User -> User에 선언된 property와 method를 그대로 물려받음
    // User: 부모 클래스, PremiumUser: 자식 클래스
    class PremiumUser extends User {
      constructor(email, birthdate, level) {
        **super(email, birthdate); // super를 통해 부모 클래스의 constructor를 호출해야 함**
        this.level = level;
      }
    
      streamMusicForFree() {
        console.log(`Free music streaming for ${this.email}`);
      }
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const pUser1 = new PremiumUser('test_p1@gmail.com', '1995-02-23', 3);
    console.log(pUser1.email);
    console.log(pUser1.birthdate);
    console.log(pUser1.level);
    pUser1.buy(item);
    pUser1.streamMusicForFree();
    ```
    

## **다형성**

- **많은 형태를 갖고 있는 성질**
- **하나의 변수가 다양한 종류의 객체를 가리킬 수 있는 걸 다형성이라고 함**
- **자식 클래스에서 부모 클래스와 동일한 `method` 이름을 짓고 사용하는 것을 오버라이딩(overriding)이라고 함**
- ex) 프리미엄 유저에게 별도의 5% 할인을 해주고자 할 때
    
    ```jsx
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        this.email = email;
        this.birthdate = birthdate;
      }
    
      // 해당 객체의 메소드
      buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }
    }
    
    // extends User -> User에 선언된 property와 method를 그대로 물려받음
    // User: 부모 클래스, PremiumUser: 자식 클래스
    class PremiumUser extends User {
      constructor(email, birthdate, level) {
        super(email, birthdate); // super를 통해 부모 클래스의 constructor를 호출해야 함
        this.level = level;
      }
    
      buy(item) {
        console.log(`${this.email} buys ${item.name} with a 5% discount`);
      }
    
      streamMusicForFree() {
        console.log(`Free music streaming for ${this.email}`);
      }
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const user1 = new User('user@gmail.com', '1992-02-02');
    const pUser1 = new PremiumUser('p_user@gmail.com', '2001-01-01');
    
    user1.buy(item);
    pUser1.buy(item);
    ```
    

## 부모 클래스의 메소드가 필요하다면?

- overriding한 메소드지만 부모 클래스의 메소드를 `super` 키워드로 호출할 수 있음
    
    ```jsx
    class User {
      // 생성자 메소드
      constructor(email, birthdate) {
        this.email = email;
        this.birthdate = birthdate;
      }
    
      // 해당 객체의 메소드
      buy(item) {
        console.log(`${this.email} buys ${item.name}`);
      }
    }
    
    // extends User -> User에 선언된 property와 method를 그대로 물려받음
    // User: 부모 클래스, PremiumUser: 자식 클래스
    class PremiumUser extends User {
      constructor(email, birthdate, level, point) {
        super(email, birthdate); // super를 통해 부모 클래스의 constructor를 호출해야 함
        this.level = level;
        this.point = point;
      }
    
      buy(item) {
        **super.buy(item); // overriding한 메소드지만 부모 클래스의 메소드를 super 키워드로 호출할 수 있음**
        this.point += item.price * 0.05;
      }
    
      streamMusicForFree() {
        console.log(`Free music streaming for ${this.email}`);
      }
    }
    
    const item = {
      name: '스웨터',
      price: 30000,
    }
    
    const user1 = new User('user@gmail.com', '1992-02-02');
    const pUser1 = new PremiumUser('p_user@gmail.com', '2001-01-01');
    
    user1.buy(item);
    pUser1.buy(item);
    ```
    
- [ ]  11.  다형성 적용해보기

## instanceof 연산자

- 어떤 `class`로 생성된 `property`인지 `instanceof`로 확인할 수 있음
- 자식 클래스로 만든 객체는 부모 클래스로 만든 객체로도 인정됨

```jsx
class User {
  // 생성자 메소드
  constructor(email, birthdate) {
    this.email = email;
    this.birthdate = birthdate;
  }

  // 해당 객체의 메소드
  buy(item) {
    console.log(`${this.email} buys ${item.name}`);
  }
}

// extends User -> User에 선언된 property와 method를 그대로 물려받음
// User: 부모 클래스, PremiumUser: 자식 클래스
class PremiumUser extends User {
  constructor(email, birthdate, level, point) {
    super(email, birthdate); // super를 통해 부모 클래스의 constructor를 호출해야 함
    this.level = level;
    this.point = point;
  }

  buy(item) {
    super.buy(item); // overriding한 메소드지만 부모 클래스의 메소드를 super 키워드로 호출할 수 있음
    this.point += item.price * 0.05;
  }

  streamMusicForFree() {
    console.log(`Free music streaming for ${this.email}`);
  }
}

const item = {
  name: '스웨터',
  price: 30000,
}

const user1 = new User('user1@gmail.com', '1992-02-23'); 
const user2 = new User('user2@gmail.com', '1934-02-23'); 
const user3 = new User('user3@gmail.com', '1956-02-23');

const pUser1 = new PremiumUser('p_user1@naver.com', '1985-02-23');
const pUser2 = new PremiumUser('p_user2@naver.com', '1945-02-23');
const pUser3 = new PremiumUser('p_user3@naver.com', '1966-02-23');

const users = [user1, pUser1, user2, pUser2, user3, pUser3];

users.forEach((user) => {
  console.log(user instanceof PremiumUser);
});
```

## static 프로퍼티와 static 메소드

- `static property`, `static method` → 클래스에 직접적으로 딸려 있는 프로퍼티와 메소드
- `constructor`도 없고 생성될 객체를 나타내는 `this` 같은 키워드도 없음
- `static`을 붙으면 별도의 객체를 생성하지 않아도 `property`와 `method`를 사용할 수 있음
    
    ```jsx
    class Math {
      static PI = 3.14;
    
      static getCircleArea(radius) {
        return Math.PI * radius * radius;
      }
    }
    
    Math.PI = 3.141592;
    Math.getRectangleArea = function (width, height) {
      return width * height;
    }
    
    console.log(Math.PI);
    console.log(Math.getCircleArea(5));
    console.log(Math.getRectangleArea(4, 5));
    
    console.log(Date.now());
    ```
    

- [ ]  14, 15, 16 추후 체크 필요