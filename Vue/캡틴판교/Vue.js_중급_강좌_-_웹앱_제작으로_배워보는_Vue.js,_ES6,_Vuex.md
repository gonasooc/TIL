# *** **리팩토링 props, emit 부분 복습 필요함**

### Todo App - 프로젝트 구현

**컴포넌트 생성 및 등록하기**

- 외부의 vue 파일을 컴포넌트로 가져오기

```jsx
<template>
  <div id="app">
    <!-- 개인 선호도에 따라 파스칼 케이스로 작성해도 무방 -->
    <TodoHeader></TodoHeader>
    <TodoInput></TodoInput>
    <TodoList></TodoList>
    <TodoFooter></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  components: {
    // 컴포넌트 태그명: 컴포넌트 내용
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter,
  }  
}
</script>

<style>

</style>
```

template 안에서는 케밥 케이스를 사용하도록 스타일 가이드에 강력하게 권고가 되어 있긴 하지만, 다른 html 태그와 구분이 안 될 수도 있어서 개인 선호에 따라 파스칼 케이스를 사용해도 무방함 (캡틴판교 코멘트)

**파비콘, 아이콘, 폰트, 반응형 태그 설정하기**

- 반응형 태그 설정하기

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

- 파비콘 생성하기

[Favicon & App Icon Generator](https://www.favicon-generator.org/)

**TodoHeader 컴포넌트 구현**

- 싱글 파일 컴포넌트 하위 style에서 scoped를 사용하면 해당 template에서만 style 사용 가능

[범위 CSS](https://vue-loader-v14.vuejs.org/kr/features/scoped-css.html)

- 전역으로 관리하고자 하면 scss 같은 전처리기 사용

**TodoInput 컴포넌트의 할 일 저장 기능 구현**

- input 삽입 후 선언된 data와 v-model 양방향 바인딩
- button 삽입 후 해당 버튼에 click method → method에는 localStorage에 저장 후 초기화하는 로직
- localStorage.setItem을 사용, key, value값을 정해서 해당 localStorage에 저장

```jsx
<template>
  <div>
    <input type="text" v-model="newTodoItem">
    <button v-on:click="addTodo">add</button>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      newTodoItem: ""
    }
  },
  methods: {
    addTodo: function() {
      console.log(this.newTodoItem);
      // 저장하는 로직 localStorage.setItem('key', 'value');
      localStorage.setItem(this.newTodoItem, this.newTodoItem);
      this.newTodoItem = "";
    }
  }
}
</script>

<style>

</style>
```

 **TodoInput 컴포넌트 코드 정리 및 UI 스타일링**

- this를 사용하면 data와 method에 접근 가능함 → 같은 인스턴스를 가리키기 때문
- 좀 더 명확한 method 구분을 위해 addTodo와 clearTodo를 분리
- click과 더불어 input 안에서 enter 눌렀을 때 저장되게끔 하기 위해 v-on:keyup.enter 사용

```jsx
<template>
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo">
    <span class="addContainer" v-on:click="addTodo">
      <i class="fas fa-plus addBtn"></i>
    </span>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      newTodoItem: ""
    }
  },
  methods: {
    addTodo: function() {
      console.log(this.newTodoItem);
      // 저장하는 로직 localStorage.setItem('key', 'value');
      localStorage.setItem(this.newTodoItem, this.newTodoItem);
      this.clearInput()
    },
    clearInput: function() {
      this.newTodoItem = "";      
    }
  }
}
</script>

<style scoped>
  input:focus {
    outline: none;
  }

  .inputBox {
    background: #fff;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }

  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }

  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
    cursor: pointer;
  }

  .addBtn {
    color: #fff;
    vertical-align: middle;
  }
</style>
```

**TodoList 컴포넌트의 할 일 목록 표시 기능 구현**

- localStorage에 있는 data를 별도로 선언한 data에 담아주는 작업이 필요함

```jsx
<template>
  <div>
    <ul>
      <li
        v-for="(todoItem, idx) in todoItems"
        v-bind:key="idx"
      >
      {{ todoItem }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      todoItems: []
    }
  },
  created: function() {
    **if(localStorage.length > 0) {
      for(let i = 0; i < localStorage.length; i++) {
        if(localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          this.todoItems.push(localStorage.key(i));
        }
      }
    }**
  }
}
</script>

<style>

</style>
```

**TodoList 컴포넌트 할 일 삭제 기능 구현**

- 해당 반복문의 index값을 매개변수로 선언해서 해당 index를 기준으로 localStorage의 removeItem, splice를 활용해서 삭제 기능 구현
- localStorage.removeItem은 localStorage 내에서 삭제, splice는 화면딴에서 즉각적으로 반영되는 걸 보여주기 위해서
- 데이터(localStorage나 API 등)와 연동해서 화면 구현할 때 항상 데이터의 변동과 화면의 변동을 나눠서 생각할 것 (window.reload를 할 것이 아니라 데이터 변동과 동시에 화면도 변동하게 해주면 됨)

```jsx
removeTodo: function(todoItem, idx) {
  console.log(todoItem, idx);
  localStorage.removeItem(todoItem);
  // 새로운 배열을 부여 / slice는 기존 배열을 유자한채 변경
	this.todoItems.splice(idx, 1);
}
```

**TodoList 컴포넌트의 할 일 완료 기능 구현**

- 저장 로직 변경

```jsx
addTodo: function() {
  **let obj = {completed: false, item: this.newTodoItem}**
  // 저장하는 로직 localStorage.setItem('key', 'value');
  // localStorage.setItem(this.newTodoItem, obj); // [object Object] -> value 확인이 안 됨
  **localStorage.setItem(this.newTodoItem, JSON.stringify(obj));**
  this.clearInput()
},
```

- 리스트에 담는 로직 변경

```jsx
created: function() {
  if(localStorage.length > 0) {
    for(let i = 0; i < localStorage.length; i++) {
      if(localStorage.key(i) !== 'loglevel:webpack-dev-server') {
        **this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));**
        // this.todoItems.push(localStorage.key(i));
      }
    }
  }
},
```

- 토글 형식으로 완료 체크하는 로직

```jsx
toggleComplete: function(todoItem) {
  todoItem.completed = !todoItem.completed;
  // 로컬 스토리지의 데이터를 갱신하는 로직(update API가 없기 때문에)
  localStorage.removeItem(todoItem.item);
  localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
}
```

**[리팩토링] 할 일 목록 표시 기능**

```
v-bind:내려보낼 props 속성 이름 = "현재 위치의 컴포넌트의 data 속성 이름"
ex) <TodoList **v-bind:propsdata="todoItems"**></TodoList>
```

**[리팩토링] 할 일 추가 기능**

```
<TodoInput
	**v-on:하위 컴포넌트에서 발생시킨 이벤트 이름="현재 컴포넌트의 메소드 이름"**
>
</TodoInput>
```