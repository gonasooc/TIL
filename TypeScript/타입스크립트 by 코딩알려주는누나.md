[타입스크립트 진짜 쉽게 설명해줌 | 이거보고도 이해 안되면 집가라](https://youtu.be/GHHUjITelsA)

### ts에서 사용 가능한 타입

- number, string, boolean, null, undefined, any(이것도 저것도 아닌 그 모든 것)

```tsx
let a: number = 3;
a = "asdadasda"; // error

let b: string = "love you";
b = true; // error
```

### **any type - 내가 어떤 데이터가 들어올지 모를 때**

- 하지만 사용을 줄어야 함 → any type을 남발하면 ts를 쓰는 의미가 무색함
- 보통 프로젝트에서는 any type은 거의 쓰지 않음

### 최소한 글자나 숫자 정도로 타입을 좁힐 수 있을 때

```tsx
let d: number | string = "asdadad";
d = 1232123;
d = null;
```

### Array

```tsx
let e: string[] = ["apple", "mongo"];
e.push(123); // error
```

### 함수

- number의 인자를 받아서 return value를 number로 한다
  ```tsx
  function addNumber(a: number, b: number): number {
    return a + b;
  }
  ```

### 컴파일

- `tsc index.ts` → 같은 경로에 js 파일이 생성됨
- 컴파일할 때 세팅값을 설정할 수 있음 → tsconfig.json

[코딩에 진심인 사람을 위해 준비한 리액트 타입스크립트 | 실제 회사에서 쓰는 레벨 ver](https://youtu.be/V9XLst8UEtk)

- Creating a TypeScript app
  - React + TypeScript → `npx create-react-app my-app --template typescript`
  - 전부 .tsx로 세팅됨
- tsconfig.json

  ```json
  {
    "compilerOptions": {
      "target": "es6",
      "lib": ["dom", "dom.iterable", "esnext", "es6"],
      "allowJs": true,
      "skipLibCheck": true,
      "esModuleInterop": true,
      "allowSyntheticDefaultImports": true,
      "strict": true,
      "forceConsistentCasingInFileNames": true,
      "noFallthroughCasesInSwitch": true,
      "module": "esnext",
      "moduleResolution": "node",
      "resolveJsonModule": true,
      "isolatedModules": true,
      "noEmit": true,
      "jsx": "react-jsx"
    },
    "include": ["src"]
  }
  ```

- 함수에 매개변수도 타입을 지정해줘야 함, 함수의 return 값도 타입 지정 필요
  - App.tsx의 경우 해당 컴포넌트를 return 하는데 어떤 타입을 지정해줘야 하나
  - React에서 이미 그 타입은 지정해둠
- React의 경우 함수 컴포넌트(Function Compoment)는 `React.FC` 로 타입 지정
  -
  ```
  import React from "react";

  const Store: React.FC = () => {
    return <div>Store</div>;
  };

  export default Store;
  ```
- react snippet → `rafce`
- ts는 type에 굉장히 strict함 → props 같은 것도 일일히 type 설정 필요함
  ```
  Type '{ info: Restaurant; }' is not assignable to type 'IntrinsicAttributes'.
    **Property 'info' does not exist on type** 'IntrinsicAttributes'.ts(2322)
  ```
- 하나로 정의하게 어려운 object 안에 여러 가지 property가 들어가는 경우, 즉 타입을 하나로 규정할 수 없는 경우 type을 만들어줄 수 있음
  - type을 만드는 건 interface와 type 이렇게 두 가지 방법으로 만들 수 있음(큰 차이는 없음, 사용할 수 있는 메소드가 좀 다르고 syntax가 좀 다름)
- type으로 정의하는 방법 → type의 type도 만들 수 있음(ex) Address)
  ```tsx
  // let data = {
  //   name: "누나네 식당",
  //   category: "western",
  //   address: {
  //     city: "incheon",
  //     detail: "somewhere",
  //     zipCode: "123132",
  //   },
  //   menu: [{ name: "rose pasta", price: 2000, category: "pasta" }],
  // };

  export type Restaurant = {
    name: string;
    category: string;
    address: Address;
    menu: Menu[];
  };

  export type Address = {
    city: string;
    detail: string;
    zipCode: Number;
  };

  export type Menu = {
    name: string;
    price: number;
    category: string;
  };
  ```
- useState의 경우 generic 문법을 통해 선언할 수 있음
  ```tsx
  const [myRestaurant, setMyRestaurant] = useState<Restaurant>(data);
  ```
  - useState라는 함수는 type을 하나로 규정할 수 없고, 함수를 호출할 때 type을 정하는데 그때 사용하는 문법이 generic → 어떠한 클래스 혹은 함수에서 사용할 타입을 그 함수나 클래스를 사용할 때 결정하는 프로그래밍 기법을 말함
- props로 데이터를 내려주는 경우, props 또한 type을 지정해줘야 함
- props로 함수를 내려주는 경우, 함수의 매개변수와 return값의 type을 지정해줘야 함
  ```tsx
  // App.tsx

  import React, { useState } from "react";
  import logo from "./logo.svg";
  import "./App.css";
  import Store from "./Store";
  import { Address, Restaurant } from "./model/resturant";

  let data: Restaurant = {
    name: "누나네 식당",
    category: "western",
    address: {
      city: "incheon",
      detail: "somewhere",
      zipCode: 123132,
    },
    menu: [{ name: "rose pasta", price: 2000, category: "PASTA" }],
  };
  const App: React.FC = () => {
    const [myRestaurant, setMyRestaurant] = useState<Restaurant>(data);

    const changeAddress = (address: Address) => {
      setMyRestaurant({ ...myRestaurant, address: address });
    };

    return (
      <div className="App">
        <Store info={myRestaurant} changeAddress={changeAddress} />
      </div>
    );
  };

  export default App;
  ```
  ```tsx
  // Store.tsx

  import React from "react";
  import { Address, Restaurant } from "./model/resturant";

  interface OwnProps {
    info: Restaurant;
    changeAddress(address: Address): void; // 함수의 경우
  }

  const Store: React.FC<OwnProps> = (props) => {
    return <div>{props.info.name}</div>;
  };

  export default Store;
  ```
- 기존에 정의된 type에 뭔가를 더 추가하고 싶은 경우 extends 사용
  ```tsx
  // App.tsx

  import React, { useState } from "react";
  import "./App.css";
  import Store from "./Store";
  import { Address, Restaurant } from "./model/resturant";
  import BestMenu from "./BestMenu";

  let data: Restaurant = {
    name: "누나네 식당",
    category: "western",
    address: {
      city: "incheon",
      detail: "somewhere",
      zipCode: 123132,
    },
    menu: [{ name: "rose pasta", price: 2000, category: "PASTA" }],
  };
  const App: React.FC = () => {
    const [myRestaurant, setMyRestaurant] = useState<Restaurant>(data);

    const changeAddress = (address: Address) => {
      setMyRestaurant({ ...myRestaurant, address: address });
    };

    const showBestMenuName = (name: string) => {
      return name;
    };

    return (
      <div className="App">
        <Store info={myRestaurant} changeAddress={changeAddress} />
        <BestMenu
          name="불고기피자"
          category="피자"
          price={1000}
          showBestMenuName={showBestMenuName}
        />
      </div>
    );
  };

  export default App;
  ```
  ```tsx
  // BestMenu.tsx

  import React from "react";
  import { Menu } from "./model/resturant";

  interface OwnProps extends Menu {
    showBestMenuName(name: string): string;
  }

  const BestMenu: React.FC<OwnProps> = ({
    name,
    price,
    category,
    showBestMenuName,
  }) => {
    return <div>{name}</div>;
  };

  export default BestMenu;
  ```
  - interface 대신에 type 쓰는 경우
    ```tsx
    type OwnProps = Menu & {
      showBestMenuName(name: string): string;
    };
    ```
- type과 interface의 기능적 차이

  - type의 주요 기능 중 기존 타입에서 특정 타입만 뺄 수 있음
    - 직접 제외한 type을 직접 선언할 수도 있고,
      ```tsx
      // resturant.js

      export type AddressWithoutZip = Omit<Address, "zipCode">;

      // type AddressWithoutZip = {
      //   city: string;
      //   detail: string;
      // }
      ```
    - 사용하는 영역에서 바로 뺄 수도 있음
      ```tsx
      // App.tsx

      import React, { useState } from "react";
      import "./App.css";
      import Store from "./Store";
      import { Address, Restaurant } from "./model/resturant";
      import BestMenu from "./BestMenu";

      let data: Restaurant = {
        name: "누나네 식당",
        category: "western",
        address: {
          city: "incheon",
          detail: "somewhere",
          zipCode: 123132,
        },
        menu: [{ name: "rose pasta", price: 2000, category: "PASTA" }],
      };
      const App: React.FC = () => {
        const [myRestaurant, setMyRestaurant] = useState<Restaurant>(data);

        const changeAddress = (address: Address) => {
          setMyRestaurant({ ...myRestaurant, address: address });
        };

        const showBestMenuName = (name: string) => {
          return name;
        };

        return (
          <div className="App">
            <Store info={myRestaurant} changeAddress={changeAddress} />
            <BestMenu
              name="불고기피자"
              category="피자"
              showBestMenuName={showBestMenuName}
            />
          </div>
        );
      };

      export default App;
      ```
      ```tsx
      // BestMenu.tsx

      import React from "react";
      import { Menu } from "./model/resturant";

      interface OwnProps extends Omit<Menu, "price"> {
        showBestMenuName(name: string): string;
      }

      const BestMenu: React.FC<OwnProps> = ({
        name,
        category,
        showBestMenuName,
      }) => {
        return <div>{name}</div>;
      };

      export default BestMenu;
      ```
  - 반대로 한 타입만 가져올 수 있음
    ```tsx
    export type AddressWithoutZip = Omit<Address, "zipCode">;
    export type RestaurantOnlyCategory = Pick<Restaurant, "category">;
    ```
    - 다만 Omit 대신에 옵셔널 체이닝을 통해 선택사항으로 만들어줄 수도 있음
      ```tsx
      export type Address = {
        city: string;
        detail: string;
        zipCode?: Number;
      };
      ```
      https://ko.javascript.info/optional-chaining

- api response 값도 type으로 정해놓을 수 있음
