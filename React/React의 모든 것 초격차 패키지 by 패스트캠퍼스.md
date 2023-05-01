# Part 1. React의 기초

### 02. DOM 다루기 Element 생성하기

[CodeSandbox: Online Code Editor and IDE for Rapid Web Development](https://codesandbox.io/)

by Vanilla

```jsx
<body>
  <div id="root"></div>
  <script>
    const rootElement = document.getElementById("root");
    const element = document.createElement("h1");
    element.textContent = "Hello, world!";
    rootElement.appendChild(element);
  </script>
</body>
```

by React

```jsx
<script>
  const rootElement = document.getElementById("root");
  // const element = document.createElement("h1");
  const element = React.createElement("h1", { children: "Hello, World!" });
  // element.textContent = "Hello, world!";

  ReactDOM.render(element, rootElement);
  // rootElement.appendChild(element);
</script>
```

### 03. JSX과 Babel, JSX 다루기

- JSX: 문자도 HTML도 아닌 JavaScript의 확장 문법

```
const element = <h1>Hello, world!</h1>;
```

- JSX 사용을 위해서는 Babel(compiler)이 필요함

```jsx
<script type="text/babel">
  const rootElement = document.getElementById("root");

  const text = "Hello, world!";
  const titleClassName = "title123";
  const props = { className: titleClassName, children: text };
  // const custom = <h1 className={titleClassName}>{text}</h1>;
  // const custom = <h1 className={titleClassName} children={text} />;
  const custom = <h1 {...props} />; // spread 연산자
  const element = custom;
  ReactDOM.render(element, rootElement);
  // rootElement.appendChild(element);
</script>
```

### 04. 멀티 Element 생성하기

```jsx
<script type="text/babel">
  const rootElement = document.getElementById("root");
  const element = (
    <div
      className="box"
      children={[
        React.createElement("h1", null, "Hi"),
        React.createElement("h3", null, "Bye"),
        React.createElement("h5", null, "Children")
      ]}
    />
  );
  ReactDOM.render(element, rootElement);
</script>
```

- root 밑에 바로 생성(React.Fragment)

```jsx
<script type="text/babel">
  const rootElement = document.getElementById("root");
  const element = (
    <**React.Fragment // React.Fragment 없이 빈 태그로 열고 닫으면 그게 동일하게 적용**
      className="box"
      children={[
        React.createElement("h1", null, "Hi"),
        React.createElement("h3", null, "Bye"),
        React.createElement("h5", null, "Children")
      ]}
    />
  );
  ReactDOM.render(element, rootElement);
</script>
```

```jsx
<script type="text/babel">
  const rootElement = document.getElementById("root");
  const element = (
    <> // **React.Fragment 없이 빈 태그로 열고 닫으면 그게 동일하게 적용**
      <h1>Hi</h1>
      <h3>Bye</h3>
      <h5>Children</h5>
    </>
  );
  ReactDOM.render(element, rootElement);
</script>
```

### 04. Element 찍어내기

```jsx
<script type="text/babel">
  const rootElement = document.getElementById("root");
  const paint = (title, description) => (
    <>
      <h1>{title}</h1>
      <h3>{description}</h3>
    </>
  );
  const Paint = ({ title, description }) => ( // Props
    <>
      <h1>{title}</h1>
      <h3>{description}</h3>
    </>
  );
  const element = (
    <>
      <Paint title="Good" description="good" />
      <Paint title="Bad" description="bad" />
      <Paint title="So so" description="so so" />
      {paint("Bad", "bad")}
      {paint("So so", "so so")}
    </>
  );
  ReactDOM.render(element, rootElement);
</script>
```

### 05. JS와 JSX 섞어쓰기