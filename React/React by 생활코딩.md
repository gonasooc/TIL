### **React 2022년 개정판 - 1. 수업소개**

- React의 핵심적인 역할은 사용자 정의 태그를 만드는 것
- 사용자 정의 태그를 부품으로서 활용하는 것(컴포넌트 단위)

### React 2022년 개정판 - 2. 실습환경구축

- node.js 설치 후 해당 프로젝트 폴더에서 `npx create-react-app .`
- `npm start`

### React 2022년 개정판 - 3. 소스코드수정방법

- `src\index.js` → 입구
    - index.js에는 여러 가지 기본적인 설정들이 들어가 있음
- root라는 id는 `public\index.html`에 정의되어 있음

**배포**

- 배포판을 만드는 건 `npm run build` → 배포판은 띄어쓰기나 배포에 불필요한 요소 제거
- 빌드된 걸 실행할 때는 `serve -s build` → serve는 웹서버고 옵션 -s는 사용자가 어떤 경로로 들어오든 index.html 파일을 서비스하게 됨
- `npx serve -s build` 를 통해서 실행한 건 개발을 위한 환경이 아니고 실제 배포 환경

### **React 2022년 개정판 - 4. 컴포넌트만들기**

- React는 사용자 정의 태그를 만드는 기술이다. 이것이 React의 본질이다.
- 사용자 정의 태그(컴포넌트)를 만들 때는 함수로 정의하면 됨
- React에서 함수로 컴포넌트를 만들 때는 **반드시 대문자를 명시적으로 사용해줘야 함**
    
    ```jsx
    import './App.css';
    
    function Header() {
      return <header>
      <h1><a href="/">React</a></h1>
    </header>
    }
    function Nav() {
      return <nav>
      <ol>
        <li><a href="/read/1">html</a></li>
        <li><a href="/read/2">css</a></li>
        <li><a href="/read/3">js</a></li>
      </ol>
    </nav>
    }
    function Article() {
      return <article>
      <h2>Welcome</h2>
      Hello, WEB
    </article>
    }
    function App() {
      return (
        <div className="App">
          **<Header></Header>
          <Nav></Nav>
          <Article></Article>**
        </div>
      );
    }
    
    export default App;
    ```
    

### **React 2022년 개정판 - 5. props**

- **html의 attribute와 같은 걸 컴포넌트 영역에선 prop이라고 부름**
- 정적 binding을 할 땐 `title="WEB"` 이런 식으로 작성하지만 동적 binding을 위해서는 `title={title}` 이런 식으로 중괄호 사용
- 해당 컴포넌트에 parameter로 props를 넣어준 후 해당 이름 선언해준 후 부모에 뿌려주면 됨
    
    ```jsx
    function Header(**props**) {
      return <header>
      <h1><a href="/">{ **props.title** }</a></h1>
    </header>
    }
    
    return (
      <div className="App">
        <Header **title="WEB"**></Header>
        <Nav topics={topics}></Nav>
        <Article title="Welcome" body="Hello, WEB"></Article>
      </div>
    );
    ```
    

- object형의 자료를 props로 넣어주고자 할 때도 {} 사용
    
    ```jsx
    import './App.css';
    
    function Header(props) {
      return <header>
      <h1><a href="/">{ props.title }</a></h1>
    </header>
    }
    **function Nav(props) {
      const lis = []
      for(let i = 0; i < props.topics.length; i++) {
        let t = props.topics[i];
        lis.push(<li key={t.id}><a href={'/read/'+t.id}>{ t.title }</a></li>)    
      }
      return <nav>
      <ol>
        {lis}
      </ol>
    </nav>
    }**
    **function Article(props) {
      return <article>
      <h2>{ props.title }</h2>
      { props.body }
    </article>
    }**
    function App() {
      const topics = [
        {id: 1, title: 'html', body: 'html is...'},
        {id: 2, title: 'css', body: 'css is...'},
        {id: 3, title: 'javascript', body: 'javascript is...'},
      ]
      return (
        <div className="App">
          <Header title="WEB"></Header>
          <Nav topics={topics}></Nav>
          <Article title="Welcome" body="Hello, WEB"></Article>
        </div>
      );
    }
    
    export default App;
    ```
    

- **React는 자동으로 생성되는 태그(반복문을 통해서)의 경우 각각 고유의 key값을 설정해줘야 함 → 이건 Vue에서도 동일함**

### **React 2022년 개정판 - 6. 이벤트**

- React 안에서 function 안쪽의 html은 유사HTML → 유사HTML을 최종적으로 랜더링해주는 것
- event 객체를 함수의 첫번째 parameter로 넘겨줌
    
    ```jsx
    import './App.css';
    
    function Header(props) {
      return <header>
      <h1><a href="/" onClick={function(event){
        event.preventDefault();
        props.onChangeMode();
      }}>{ props.title }</a></h1>
    </header>
    }
    function Nav(props) {
      const lis = []
      for(let i = 0; i < props.topics.length; i++) {
        let t = props.topics[i];
        lis.push(<li key={t.id}><a id={t.id} href={'/read/'+t.id} onClick={function(event){
          event.preventDefault();
          props.onChangeMode(event.target.id);
        }}>{ t.title }</a></li>)    
      }
      return <nav>
      <ol>
        {lis}
      </ol>
    </nav>
    }
    function Article(props) {
      return <article>
      <h2>{ props.title }</h2>
      { props.body }
    </article>
    }
    function App() {
      const topics = [
        {id: 1, title: 'html', body: 'html is...'},
        {id: 2, title: 'css', body: 'css is...'},
        {id: 3, title: 'javascript', body: 'javascript is...'},
      ]
      return (
        <div className="App">
          <Header title="WEB" onChangeMode={function(){
            alert('Header');
          }}></Header>
          <Nav topics={topics} onChangeMode={function(id){
            alert(id);
          }}></Nav>
          <Article title="Welcome" body="Hello, WEB"></Article>
        </div>
      );
    }
    
    export default App;
    ```
    

### **React 2022년 개정판 - 7. state**

- **Props: 컴포넌트를 사용하는 외부자를 위한 데이터 / State: 컴포넌트를 만드는 내부자를 위한 데이터**
- 최상단에 `useState`를 import하고 변수를 담을 때 useState를 사용하겠다고 선언하면 `'useState`를 사용할 수 있음
- **useState의 0번째 원소는 상태의 값을 읽을 때 쓰는 데이터, 1번째 데이터는 그 상태의 값을 변경할 때 사용**
    
    ```jsx
    const _mode = useState('WELCOME');
    const mode = _mode[0]; // WELCOME
    ```
    
- useState의 인자는 그 state의 초기값
- 주로 축약해서 이렇게 선언함
    
    ```jsx
    // const _mode = useState('WELCOME');
    // const mode = _mode[0];
    // const setMode = _mode[1];
    const [mode, setMode] = useState('WElCOME');
    ```
    
- 일반적으로 변수를 선언해서 그 변수의 값에 따라 화면을 다시 그리고자 해도 App는 한번만 그려짐
- React에서는 `useState`를 선언해서 `setMode`, 즉 state를 바꾸고자 하는 값을 바꾸면 App을 다시 그려줌
    
    ```jsx
    import './App.css';
    import {useState} from 'react';
    
    function Header(props) {
      return <header>
      <h1><a href="/" onClick={function(event){
        event.preventDefault();
        props.onChangeMode();
      }}>{ props.title }</a></h1>
    </header>
    }
    function Nav(props) {
      const lis = []
      for(let i = 0; i < props.topics.length; i++) {
        let t = props.topics[i];
        lis.push(<li key={t.id}><a id={t.id} href={'/read/'+t.id} onClick={function(event){
          event.preventDefault();
          props.onChangeMode(Number(event.target.id));
        }}>{ t.title }</a></li>)    
      }
      return <nav>
      <ol>
        {lis}
      </ol>
    </nav>
    }
    function Article(props) {
      return <article>
      <h2>{ props.title }</h2>
      { props.body }
    </article>
    }
    function App() {
      // const _mode = useState('WELCOME');
      // const mode = _mode[0];
      // const setMode = _mode[1];
      const [mode, setMode] = useState('WElCOME');
      const [id, setId] = useState(null);
      console.log(mode);
      const topics = [
        {id: 1, title: 'html', body: 'html is...'},
        {id: 2, title: 'css', body: 'css is...'},
        {id: 3, title: 'javascript', body: 'javascript is...'},
      ]
      let content = null;
      if(mode === 'WELCOME') {
        content = <Article title="Welcome" body="Hello, WEB"></Article>
      } else if(mode === 'READ') {
        let title, body = null;
        for(let i=0; i<topics.length; i++) {
          console.log(topics[i].id, id);
          if(topics[i].id === id) {
            title = topics[i].title;
            body = topics[i].body;
          }
        }
        content = <Article title={title} body={body}></Article>
      }
      return (
        <div className="App">
          <Header title="WEB" onChangeMode={function(){
            setMode('WELCOME');
          }}></Header>
          <Nav topics={topics} onChangeMode={function(_id){
            setMode('READ');
            setId(_id);
          }}></Nav>
          {content}
        </div>
      );
    }
    
    export default App;
    ```
    

### **React 2022년 개정판 - 8. Create**

```jsx
// import logo from './logo.svg';
import './App.css';
import {useState} from 'react'

function Nav(props) {
  const lis = [];
  for(let i = 0; i < props.topics.length; i++) {
    let t = props.topics[i];
    lis.push(<li key={t.id}>
      <a id={t.id} href={'/read/'+t.id} onClick={(event) => {
        event.preventDefault();
        props.onChangeMode(Number(event.target.id));
      }}>{t.title}</a>
    </li>)
  }

  return (
    <nav>
      <ol>
        {lis}
      </ol>
    </nav>
  )
}

function Article(props) {
  return (
    <article>
      <h2>{props.title}</h2>
      {props.body}
    </article>
  )
}

function Header(props) {
  return (
    <header>
      <h1><a href="/" onClick={(event) => {
        event.preventDefault();
        props.onChangeMode();
      }}>{props.title}</a></h1>
    </header>
  )
}

function Create(props) {
  return <article>
    <h2>Create</h2>
    <form onSubmit={function(event) {
      event.preventDefault();
      const title = event.target.title.value;
      const body = event.target.body.value;
      props.onCreate(title, body)
    }}>
      <p><input type="text" name="title" placeholder='title' /></p>
      <p><textarea name="body" placeholder='body'></textarea></p>
      <p><input type="submit" value="Create"></input></p>
    </form>
  </article>
}

function App() {
  // const _mode = useState('WELCOME');
  // const mode = _mode[0];
  // const setMode = _mode[1];
  const [mode, setMode] = useState('WELCOME');
  const [id, setId] = useState(null);
  const [nextId, setNextId] = useState(4);
  const [topics, setTopics] = useState([
    {id: 1, title: 'html', body: 'html is ...'},
    {id: 2, title: 'css', body: 'css is ...'},
    {id: 3, title: 'javascript', body: 'javascript is ...'},
  ])

  let content = null;
  if(mode === 'WELCOME') {
    content = <Article title="Welcome" body="Hello, WEB"></Article>
  } else if (mode === 'READ') {
    let title, body = null;
    for(let i = 0; i < topics.length; i++) {
      if(topics[i].id === id) {
        title = topics[i].title;
        body = topics[i].body;
      }
    }
    content = <Article title={title} body={body}></Article>
  } else if (mode === 'CREATE') {
    content = <Create onCreate={function(_title, _body) {
      const newTopic = {id: nextId, title: _title, body: _body}
      const newTopics = [...topics];
      newTopics.push(newTopic);
      setTopics(newTopics);
      setMode('READ');
      setId(nextId);
      setNextId(nextId + 1);
    }}></Create>
  }

  return (
    <div>
      <Header title="REACT" onChangeMode={() => {
        alert('Header');
        setMode('WELCOME')
      }}></Header>
      <Nav topics={topics} onChangeMode={(_id) => {
        alert(_id);
        setMode('READ');
        setId(_id);
      }}></Nav>
      {content}
      <a href="/create" onClick={event => {
        event.preventDefault();
        setMode('CREATE');
      }}>Create</a>
    </div>
  );
}

export default App;
```

**Create, Update, Delete는 리액트 어느 정도 익숙해졌을 때 리뷰**