# **섹션 0. Vue.js 소개**

## Vue 2와 Vue 3의 차이점

- 라이브러리 내부 로직 재작성 - 타입스크립트 기반으로 재작성
- 주요 개발 도구들 변경
  - 예) 뷰 개발자 도구, VSCode 플러그인(Vetur 대신 Volar), (Webpack 대신)Vite 기반 프로젝트 생성 등
- 컴포지션 API, Teleport 등 새로운 문법 지원
- 리액티비티 시스템 기반 API 변경
- 공식 문서 변경

## Vue 3 코드 작성 방법

- 옵션 API / Composition API

## Hello World(Vue.js 인스턴스)

```jsx
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  Vue.createApp({
    data() {
      return {
        message: "hi",
      };
    },
  }).mount("#app");
</script>
```

# **섹션 1. Vue.js 핵심 동작 원리**

## Vue 3 Reactivity - Proxy 소개 & Vue 3 Reactivity - 동작 원리 구현

[Proxy - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

```html
<div id="app">
  <!-- 여기에 뭐가 랜더링 된다 -->
</div>
<script>
  var data = {
    message: 10,
  };

  function render(sth) {
    var div = document.querySelector("#app");
    div.innerHTML = sth;
  }

  var app = new Proxy(data, {
    get() {
      console.log("값 접근");
    },
    set(target, prop, newValue) {
      console.log("값 갱신");
      target[prop] = newValue;
      render(newValue);
    },
  });
</script>
```

## Reactivity 차이점 - Vue 2 & Vue 3

- 관련 자료
  [Reactivity Fundamentals | Vue.js](https://vuejs.org/guide/essentials/reactivity-fundamentals.html#reactivity-fundamentals)
  [Reactivity in Depth | Vue.js](https://vuejs.org/guide/extras/reactivity-in-depth.html)
  [Reactivity in Depth — Vue.js](https://v2.vuejs.org/v2/guide/reactivity.html)
- Vue2 Reactivity 관련 이미지
  ![data.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/cbe44aa2-7fb2-4c72-86b3-711de49a1f7f/0fcb23d2-ffdc-4f02-96eb-9cd80a9e0313/data.png)
- Vue2 - `Object.defineProperty()` / Vue3 - `Proxy()`

# **섹션 2. Vue.js 기본 개념과 문법**

## Vue Instance

[Instance 🆕 | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/instance.html#인스턴스-생성)

[Creating a Vue Application | Vue.js](https://vuejs.org/guide/essentials/application.html)

```jsx
<script>
  // Vue 2
  new Vue({
    el: '#app'
  })

  // Vue 3
  Vue.createApp({
    data() {
      return {
        message: "hi",
      };
    },
  }).mount("#app");
</script>
```

## Vue Methods

[Methods | Cracking Vue.js](https://joshua1988.github.io/vue-camp/syntax/methods.html)

## Vue Directive: v-for

[List Rendering | Vue.js](https://vuejs.org/guide/essentials/list.html)

# **섹션 3. Vue.js 컴포넌트**

## Vue Component 소개

[Components | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/components.html)

## Vue Component 등록과 표시

```jsx
<!-- HTML -->
<div id="app">
  <!-- 컴포넌트 표시 -->
  <app-header></app-header>
</div>

<!-- JavaScript -->
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
  Vue.createApp({
    // 인스턴스 옵션 속성 - 옵션 API
    components: {
      // '컴포넌트 이름': 컴포넌트 내용
      "app-header": {
        template: "<h1>컴포넌트 등록</h1>",
      },
    },
  }).mount("#app");
</script>
```

## Vue Component 통신 방식

- 데이터는 아래로 흐르고(props), 이벤트(일반적인 이벤트가 아니라 데이터를 주고 받기 위한)는 위로 올라감(event emit) → 다시 데이터를 내려달라는 event emit

## Vue Component Props

[Props | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/props.html)

```jsx
<!-- HTML -->
<div id="app">
  <app-header v-bind:title="appTitle"></app-header>
</div>

<!-- JavaScript -->
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
  Vue.createApp({
    data() {
      return {
        appTitle: "프롭스 넘기기",
      };
    },
    components: {
      "app-header": {
        template: "<h1>{{ title }}</h1>",
        props: ["title"],
      },
    },
  }).mount("#app");
</script>
```

## Event Emit 소개

[Event Emit | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/event-emit.html)

## Event Emit 구현

```html
<!-- HTML -->
<div id="app">
  <!-- <app-contents v-on:이벤트이름="상위 컴포넌트의 메서드 이름"></app-contents> -->
  <app-contents v-on:refresh="showAlert"></app-contents>
</div>

<!-- JavaScript -->
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
  var appContents = {
    template: `
    <p>
      <button v-on:click="sendEvent">갱신</button>  
    </p>
    `,
    methods: {
      sendEvent() {
        this.$emit("refresh");
      },
    },
  };

  Vue.createApp({
    components: {
      "app-contents": appContents,
    },
    methods: {
      showAlert() {
        window.alert("새로고침");
      },
    },
  }).mount("#app");
</script>
```

## 같은 레벨의 컴포넌트간 데이터 전달 방법

- 직접 통신은 불가능하고, 올려준 event emit을 통해 변경된 데이터를 props로 다시 내려줌
  ```html
  <!-- HTML -->
  <div id="app">
    <app-header v-bind:app-title="message"></app-header>
    <app-contents v-on:login="receive"></app-contents>
  </div>

  <!-- JavaScript -->
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  <script>
    var appHeader = {
      template: "<h1>{{ appTitle }}</h1>",
      props: ["appTitle"],
    };

    var appContents = {
      template: `
        <p>
          <button v-on:click="sendEvent">로그인</button>
        </p>
      `,
      methods: {
        sendEvent() {
          this.$emit("login");
        },
      },
    };

    // 루트 컴포넌트
    Vue.createApp({
      data() {
        return {
          message: "로그인 하세요",
        };
      },
      components: {
        // '컴포넌트 이름': 컴포넌트 내용
        "app-header": appHeader,
        "app-contents": appContents,
      },
      methods: {
        receive() {
          this.message = "로그인 됨";
        },
      },
    }).mount("#app");
  </script>
  ```

# 섹션 4. Vue.js 템플릿 문법

## Vue 템플릿 문법(Template Syntax) 소개
