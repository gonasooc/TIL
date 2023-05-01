### :class binding할 때 이항과 삼항의 차이

- 기본적으로 이항을 사용할 때 코드 구성

```
:class="{ active: active }"
```

- 삼항을 사용하고자 할 때는 앞뒤로 중괄호가 빠져야 함

```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

### **created 통해서 addEventListener 잡았으면 beforeDestroy 통해서 제거도 필요**

```jsx
created() {
    if (['Notice', 'Faq', 'DesignStorageDetail', 'NoticeDetail'].includes(this.$route.name)) {
      window.addEventListener('scroll', this.changeHeaderType)
    }
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.changeHeaderType)
  },
```

### **랜더링 전에 구분문자 등을 감추기 위해선 v-cloak 사용**

### **constant 쪽에 정의해놓은 애들 가져다 쓸 때**

```jsx
$constants.shType[item.ra_name] // HTML 안에서
this.$constants.shType // script 안에서
```

### **v-model.trim → 앞뒤 공백 제거, v-model.number → 숫자로 자동 형변환**

# Vue swiper 관련 이슈(cdn일 경우)

### vue-awesome-swiper 사용 시 참고사항

- 스크립트 작성 순서 (swiper.min.js -> vue.min.js -> vue-awesome-swiper.js) → 항상 vue 밑에 awesome 선언해줘야 하고 최상단에 swiper (awesome-swiper 있더라도 swiper 있어야 함)
    
    ```html
    <script src="../assets/js/vendor/swiper.min.js"></script>
    <script src="../assets/js/vendor/vue.min.js"></script>
    <script src="../assets/js/vendor/vue-awesome-swiper.js"></script>
    ```
    

- 주의사항 - Vue 사용하는 곳 상단에 script로 VueAwesomeSwiper를 선언해줘야 함
    
    ```jsx
    <script>
      Vue.use(VueAwesomeSwiper);
    </script>
    ```
    

- 만약 Vue에서 선언해서 사용하는 것이 아닌 별도의 script(일반적으로 적용하는 방식)로 짰는데 위치를 잡지 못하는 경우
    
    ```jsx
    observer: true,
    observeParents: true,
    ```
    

[surmon-china/vue-awesome-swiper](https://github.com/surmon-china/vue-awesome-swiper)

# Swiper

@@swiper 관련 전달사항

vue 6.4.12 버전은 도트 페이지네이션이 뜨지 않는 오류가 있습니다!

참고하셔서 차후 swiper사용하실 때는 5.4.5 버전으로 다운그레이드하시고

npm i 재설치 후 main.js에는 import 'swiper/css/swiper.css' 로 하면 페이지네이션 나타나용

### swiper click 영역이 이상한 경우

- vue 환경에서 v-for 돌린 swiper-slide를 loop 설정할 경우 duplicate된 슬라이드는 click event가 안 먹고 실제 element만 click event 먹음 → 고로 v-for 돌린 거에는 loop 해제

- swiper loop slidePerView 같이 쓸 때 버그 있음
    
    [vue.js - vue-awesome-swiper loop options 오류 및 해결 테스트](https://hjh0827.tistory.com/60)
    

# moment 관련 이슈

웹에서 제대로 출력되었다 하더라도,

아이폰은 moment내에 임의 작성 숫자로 넣게되면 invalid date로 출력 → new Date(날짜) 로 변경
4월 같이 한자리 수의 날짜인 경우 04가 아닌 4 로 들어가면 그것도 invalid date로 뱉음

### 동일한 router에서 params나 query 변경 됐을 때 데이터 변경

```jsx
watch: {
// 동일한 router내에서 params 변경됐을 때 data 변경을 위한 watch
  async '$route.params.id' () {
    if (this.$route.name === 'MyTarmingRoom') {
      this.id = this.$route.params.id;
      await this.fetchData();
    }
  },
},
```

# keep-alive 관리

- 해당 router로 이동하기 전에 이전 페이지 keep
    
    ```jsx
    @click="keep('KinMain').then(() => $router.push(`/kin-detail/${answer.ki_no}`))"
    ```
    

- keep-alive 사용하게 되면 created 훅이 아니라 activated 훅으로 들어오기 때문에 해당 activated 훅에서 unkeep을 해줘야 함 → 왜냐면 다른 페이지에서 접근할 때는 초기 데이터 값으로 들어와야 하기 때문)
    
    ```jsx
    async activated () {
      this.unkeep('KinMain');
    },
    ```
    

# router 관련

- router 새창으로 열기
    
    ```jsx
    @click="
      let routeData = $router.resolve({
        path: '/demoday-info',
      });
      window.open(routeData.href, '_blank');
    "
    ```
    
- router 와일드카드
    
    ```jsx
    {
      path: '*',
      name: 'PageError',
      component: () => import('~@/views/PageError.vue'),
    },
    ```
    

### 캘린더 상단 '월' 이동하는 버튼 구현할 때 유용한 moment 사용법

```html
<div class="cal-ttl-wrap">
  <button
    class="btn-sld left"
    @click="date = $moment(date).subtract(1, 'months').format('YYYY-MM-01')"
  >
    왼쪽버튼
  </button>
  <span class="ttl">{{ $moment(date).format('YYYY MM월') }}</span>
  <button
    class="btn-sld right"
    @click="date = $moment(date).add(1, 'months').format('YYYY-MM-01')"
  >
    오른쪽버튼
  </button>
</div>

**// 해당 date를 moment로 계산해서 다시 담아줌**
```

### v-swiper를 v-for 반복을 돌리고 싶을 때

→ 별개의 danymic directive name을 지정해줘야 함

```
v-swiper:[`subImgSwiper${idx}`]
```