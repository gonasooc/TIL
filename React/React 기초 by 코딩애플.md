# **React 기초 1강 : 리액트 설치와 셋팅법 (2021+ 스타일)**

- 원하는 폴더에서 `npx create-react-app blog` (설치) → 리액트 셋팅 다 된 boilerplate 만들기 쉽게 도와주는 라이브러리
- 해당 프로젝트 폴더에서 `npm start`
- `public\index.html`에 `src\App.js`의 내용을 뿌리고 있음 → 그건 `index.js`에서 설정
    
    ```jsx
    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );
    ```
    
- public: static 파일 보관함 / src: 소스코드 보관함 / package.json: 설치한 라이브러리 목록

[React 기초 2강 : 리액트에선 HTML대신 JSX 써야함 (JSX 사용법)](https://www.youtube.com/watch?v=FqnAFX9lQPQ&list=PLfLgtT94nNq1e6tr4sm2eH6ZZC2jcqGOy&index=3)

# **React 기초 2강 : 리액트에선 HTML대신 JSX 써야함 (JSX 사용법)**

- JSX에서는 HTML 문법의 `class`가 아닌 `className`이라고 class를 부여함 → JS에서 이미 `class`라는 문법이 있기 때문에 구분을 위해서
- JSX를 통해 똑같이 HTML/CSS 작성함 → 근데 왜 React를 쓰느냐 → data binding이 수월하기 때문
- 직접적인 data binding
    
    ```jsx
    function App() {
    
      let posts = '강남 고기 맛집';
    
      return (
        <div className="App">
          <div className="black-nav">
            <p>개발 BLOG</p>
          </div>
          <span>{ posts }</span>
        </div>
      );
    }
    ```
    

- React에서 attribute 값을 binding할 때는 `import` 후에 똑같이 중괄호로 binding → (+)class명에도 가능
    
    ```jsx
    **import logo from './logo.svg';**
    import './App.css';
    
    function App() {
    
      function 함수() {
        return 100
      }
      
      return (
        <div className="App">
          <div className="black-nav">
            <p>개발 BLOG</p>
          </div>
          **<img src={ logo } />**
          <span>{ 함수() }</span>
        </div>
      );
    }
    ```
    

- JSX에서 inline style 넣고 싶을 때 object 형식으로 넣어야 함 → JS에서 사용하는 문법이 많기 때문에 케밥 케이스도 카멜 케이스로 바꿔서 사용
    
    ```jsx
    <p style={ { color: 'blue', fontSize: '20px', } }>개발 BLOG</p>
    ```
    

# **React 기초 3강 : 리액트에선 변수말고 state 만들어 쓰랬죠 (useState)**

- 데이터는 변수에 넣거나 state에 넣거나 두 가지 방법이 있음
- *`useState*('남자 코트 추천');` → React 내에 내장함수 사용 → 이렇게 입력하면 `[a, b]` 이런 형식의 `array`가 남음 → a는 기존 데이터, b는 변경 데이터
    - ES6 destructuring 문법 → `var [a, b] = [10, 100];`
    - `let **[title, ChangedTitle] =*useState*('남자 코트 추천');`
- 변수 대신에 `useState` 사용해서 state에 담는 이유는 웹이 App처럼 동적으로 동작하게 만들고 싶어서 → state가 변경되면 HTML이 자동으로 재랜더링

# **React 기초 4강 : 리액트에서 버튼에 이벤트 리스너(핸들러) 장착하는 법**

- `/* *eslint-disable* */` lint 무시
- state 직접 수정 화면딴에서 반영 안 됨 → ex) 글제목[0] = 1 (X)
- 그래서 state와 함께 만들었던 변경 state 함수를 사용함
- 변경 state 함수에서는 매개변수로 기존 state 사용
    
    ```jsx
    function App() {
    
      let [title, ChangedTitle] = useState(['남자 코트 추천', '강남 우동 맛집', '파이썬독학']);
    
      let [likeCnt, **ChangedLikeCnt**] = useState(0);
      
      let post = '강남 고기 맛집';
      
      return (
        <div className="App">
          <div className="black-nav">
            <p>개발 BLOG</p>
          </div>
          <div className="list">
            <h3>{ title[0] } <span onClick={ () => { **ChangedLikeCnt**(**likeCnt**++)} }>👍</span> { likeCnt } </h3>
            <p>2월 17일 발행</p>
            <hr />  
          </div>
          <div className="list">
            <h3>{ title[1] }</h3>
            <p>4월 17일 발행</p>
            <hr />  
          </div>
          <div className="list">
            <h3>{ title[2] }</h3>
            <p>6월 17일 발행</p>
            <hr />  
          </div>
        </div>
      );
    }
    ```
    

# **React 기초 5강 : state 맘대로 변경하는 법 (setState는 넘 옛날이고염)**

- useState의 변경함수를 사용해서 변경
- 리액트 원칙적으로 immutable data 권장하기 때문에 원본 데이터가 아닌 deep copy된 데이터를 변경해서 넣어주는 형식으로 작업함(React적인 관습
- deep copy: 값 공유가 되지 않고 완전히 새로운 데이터를 복사함(그냥 copy하면 값 공유가 일어남)
    
    ```jsx
    function changeTitle() {
      var newArray = [...title]; // deep copy(React적인 관습), 원본은 수정하지 않기에
      newArray[0] = '여자 코트 추천';
      ChangedTitle(newArray);
    }
    ```
    

# **React 기초 6강 : Component로 HTML 깔끔하게 줄이는 법**

- Component 선언은 App()와 나란히 → App()도 일종의 component
- 반복적으로 출현하는 HTML 덩어리를 component로 바꾸면 좋음
    
    ```jsx
    function Modal() { // Component의 이름 첫 자는 대문자
      return ( // return 안에는 반드시 하나의 div 안에
        <div className="modal">
          <h2>제목</h2>  
          <p>날짜</p>  
          <p>상세내용</p>  
        </div>
      )
    }
    ```