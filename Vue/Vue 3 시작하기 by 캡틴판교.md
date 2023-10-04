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
  ![data.png](../assets/9cd80a9e0313.png)
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

## Vue 템플릿 문법(Template Syntax) 소개 & Vue 디렉티브: v-if, v-show

[Template | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/template.html)

## Vue 데이터 바인딩: id, class, style

```html
<style>
  .primary {
    color: coral;
  }
</style>

<!-- HTML -->
<div id="app">
  <!-- class 바인딩 -->
  <h1>클래스 바인딩</h1>
  <div :class="textClass">데이터 바인딩 예제</div>

  <!-- id 바인딩 -->
  <h1>아이디 바인딩</h1>
  <section :id="sectionId" :style="sectionStyle">반갑습니다.</section>
</div>

<!-- JavaScript -->
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
<script>
  Vue.createApp({
    data() {
      return {
        textClass: "primary",
        sectionId: "tab1",
        sectionStyle: { color: "red" },
      };
    },
  }).mount("#app");
</script>
```

# **섹션 5. Vue CLI: 뷰 프로젝트 생성 도구**

## Vue CLI 소개 및 설치

[Vue CLI](https://cli.vuejs.org/)

## Vue 프로젝트 폴더 내용 살펴보기

- `package.json`의 `dependencies`와 `devDependencies`의 차이점을 아는 것도 중요
  [NPM Module Install | 웹팩 핸드북](https://joshua1988.github.io/webpack-guide/build/npm-module-install.html#개발용-라이브러리와-배포용-라이브러리-구분하기)
- Eslint와 Prettier
  [Vue.js 개발 생산성을 높여주는 도구 3가지](https://joshua1988.github.io/web-development/vuejs/boost-productivity/)
- `vue.config.js`는 Vue의 설정 파일이고 Vue CLI API 문서를 참고해서 변경 가능
  [Configuration Reference | Vue CLI](https://cli.vuejs.org/config/#vue-config-js)

# **섹션 6. Vue 싱글 파일 컴포넌트**

## Vue Single File Component 소개

[Single File Components | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/sfc.html)

## App 컴포넌트

- `vda` → vue data 영역 선언 snippet
- style scoped 된 것과 아닌 것의 차이 알아둘 것

## 싱글 파일 컴포넌트 코드 작성 팁

- `vbc` → vue SPA 구조 snippet

## Vue 컴포넌트 등록 방법 및 명명 규칙

- 컴포넌트 안에 key, value 이름이 같으면 축약 가능
  ```jsx
  // components: {
    //   AppHeader: AppHeader,
    // },
    components: {
      AppHeader,
    },
  ```

# **섹션 7. Vue.js 최종 프로젝트**

## 프로젝트 생성 및 로그인 폼 UI 구성

- `v-model`을 통한 데이터 바인딩

## 폼 이벤트 제어 및 서버로 데이터 전송

- `v-on:submit.prevent`를 통해서 `event.preventDefault()` 제어 가능
- fake API를 통한 axios 테스트
  https://jsonplaceholder.typicode.com/
- HTTP 프로토콜 참고
  [프런트엔드 개발자가 알아야하는 HTTP 프로토콜 Part 1](https://joshua1988.github.io/web-development/http-part1/)

```jsx
<template>
  <div>
    <form action="" @submit.prevent="submitForm">
      <div>
        <label for="userId">ID:</label>
        <input id="userId" type="text" v-model="username" />
      </div>
      <div>
        <label for="password">PW:</label>
        <input id="password" type="password" v-model="password" />
      </div>
      <button type="submit">로그인</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    submitForm() {
      const data = {
        username: this.username,
        password: this.password,
      };
      axios
        .post("https://jsonplaceholder.typicode.com/users/", data)
        .then((response) => {
          console.log(response);
        });
    },
  },
};
</script>

<style scoped></style>
```

## Vue Composition API 코드로 변환하기

```jsx
<template>
  <div>
    <form action="" @submit.prevent="submitForm">
      <div>
        <label for="userId">ID:</label>
        <input id="userId" type="text" v-model="username" />
      </div>
      <div>
        <label for="password">PW:</label>
        <input id="password" type="password" v-model="password" />
      </div>
      <button type="submit">로그인</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";
import { ref } from "vue";

export default {
  setup() {
    const username = ref("");
    const password = ref("");

    // methods
    const submitForm = () => {
      axios
        .post("https://jsonplaceholder.typicode.com/users/", {
          username: username.value,
          password: password.value,
        })
        .then((response) => {
          console.log(response);
        });
    };

    return { username, password, submitForm };
  },

  // data() {
  //   return {
  //     username: "",
  //     password: "",
  //   };
  // },
  // methods: {
  //   submitForm() {
  //     const data = {
  //       username: this.username,
  //       password: this.password,
  //     };
  //     axios
  //       .post("https://jsonplaceholder.typicode.com/users/", data)
  //       .then((response) => {
  //         console.log(response);
  //       });
  //   },
  // },
};
</script>

<style scoped></style>
```
