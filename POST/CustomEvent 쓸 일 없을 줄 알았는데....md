### `CustomEvent` 쓸 일이 있겠어?

`CustomEvent` 사용 사례를 처음 접한 건 올여름 FEconf 발표를 준비할 때였다. 당시 같이 준비하던 발표자분이 애니메이션 구현을 위해 특정 시점에 `CustomEvent`를 사용한 사례를 공유했다. MDN 문서는 확인해 봤지만 당장 사용할 일이 없어서 흘려보냈던 기억만 남아 있었다. 실제로 업무나 사이드 프로젝트에서 사용할 일이 없기도 했다.

### 그런데 쓸 일이 실제로 일어났습니다

최근 회사에서 특정 프로젝트를 카페24 솔루션을 사용하기로 하면서 오랜만에 바닐라 자바스크립트를 통해 개발을 진행하게 되었다. 특정 스킨을 베이스로 커스터마이징하는 작업이라고 볼 수 있는데, SSO 로그인 과정을 포함해 기존 자사 앱과 통신해야 하는 부분이 있었다. 이를 위해 `localStorage`에 값을 저장해 사용하는 방식을 채택했다.

이 프로젝트는 웹과 앱(웹뷰) 양쪽 모두에서 사용되어야 했고, 웹뷰 안에서 특정 분기 처리가 필요한 케이스가 있었다. 앱 클라이언트 개발자님과 논의한 끝에 웹뷰가 열릴 때 `query parameter`로 `mall_connect_type`과 `app`을 key, value로 넘겨주기로 하였고, 해당 값을 파싱해서 `localStorage`에 저장하고 활용하기로 했다.

### 정책과 환경적인 요구사항

우선 정책상 웹뷰에서는 로그아웃을 막아야 했다. 사용자가 웹뷰를 포함해 앱 내 여러 메뉴를 오가는 기획이었기 때문에 별도의 로그아웃 없이 서비스를 유지해야 했다. 추가로 카페24 환경에서는 국문 페이지와 영문 페이지의 인증 토큰이 별도로 관리되었기 때문에 국문 페이지에서 로그인했더라도 영문 페이지로 이동할 경우 별도의 로그인이 필요했다. 어차피 웹뷰가 열릴 때 국문과 영문 페이지 중 어디로 보낼지 결정을 위한 `lang`이라는 key와 특정 value 또한 `query parameter`로 넘어오고 있었는데, 그 key를 활용해서 redirect 처리를 하고 언어 변경 메뉴는 앱에서 보이지 않게 처리하기로 결정되었다. 정책과 환경은 작업 기간과 공수를 감안해서 결정되는 것이기 때문에 정답은 없지만 해당 시점에 맞는 협의안을 논의하고 실행했다.

### 최초 설계가 아닌 `CustomEvent`를 선택한 이유

처음엔 단순히 `query parameter`를 파싱해서 `localStorage`에 담고 `DOMContentLoaded` 이벤트 발생 시점에 처리하면 되겠다고 생각했다. 그러나 카페24 대부분의 HTML 문서가 모듈로 쪼개져 있고 정책상 숨겨야 하는 요소는 SSR 된 것을 클라이언트에서 처리하기 때문에, `localStorage`를 담는 모듈과 가져와서 사용하는 모듈의 `DOMContentLoaded`가 다소 달라질 여지가 있었다. 물론 DOM에서 숨겨야 하는 요소는 사용자와의 별도의 인터렉션을 통해 노출되는 햄버거 메뉴 안에 있었기 때문에 크게 상관은 없었지만 좀 더 명확하게 처리할 필요가 있었다.

처음엔 `CustomEvent`를 생각하지 못하고 기존의 `Event`를 쭉 살펴보고 있었다. 하지만 명확히 `query parameter`를 파싱해서 `localStorage`에 담는 시점을 캐치할 만한 이벤트가 마땅치 않았다. 그 와중에 ChatGPT의 추천으로 관련 키워드를 찾았고, 문득 그제야 눈동냥, 귀동냥 했던 `CustomEvent`가 떠올랐다.

### `CustomEvent` 생성하기

`CustomEvent`는 `Event`처럼 별도의 생성자를 통해 만들 수 있다.

```jsx
<h1 id="elem">이보라님, 환영합니다!</h1>

<script>
  // 추가 정보는 이벤트와 함께 핸들러에 전달됩니다.
  elem.addEventListener("hello", function(event) {
    alert(event.detail.name);
  });

  elem.dispatchEvent(new CustomEvent("hello", {
    detail: { name: "보라" }
  }));
</script>
```

첫 번째 인수에는 생성할 이벤트 이름(`hello`)을 넣고 두 번째 인수에는 `detail`이라는 property를 추가해서 생성할 이벤트 관련 정보를 명시할 수 있다. 당연히 이벤트를 붙일 때 해당 property를 사용할 수 있고, 어떤 데이터라도 들어가는 게 가능해서 꽤 유용하다. 그리고 예시 코드상에서는 한 번에 처리했지만 `CustomEvent`를 생성했다면 반드시 `dispatchEvent` 메소드를 통해 실행시켜야 사용이 가능하다.

### 별도의 이벤트 생성 및 추가

카페24의 HTML은 모듈 단위로 쪼개져 있어서 내가 처한 상황에서는 `query parameter`를 파싱해서 `localStorage`에 담는 모듈과 이를 사용하는 모듈이 분리되어 있었다.

우선 파싱해서 담아내는 모듈의 스크립트이다.

```jsx
<script>
    document.addEventListener('DOMContentLoaded', () => {
        const queryParams = new URLSearchParams(window.location.search);
        const mallConnectType = queryParams.get('mall_connect_type');

        if(mallConnectType) {
            localStorage.setItem('mallConnectType', mallConnectType);
        }

        document.dispatchEvent(new CustomEvent('MallConnectTypeSet')); // 수신하기 위한 커스텀 이벤트
        ...
    });
</script>
```

해당 값을 파싱해서 `localStorage`에 값을 저장한 이후에 `CustomEvent`를 생성해서 `dispatchEvent`를 실행했다.

다음은 값을 가져와서 사용하는 모듈의 스크립트이다.

```jsx
<script>
    document.addEventListener('MallConnectTypeSet', () => {
        const saved_mall_connect_type = localStorage.getItem('mallConnectType');
        const logoutButton = document.getElementById('customSidebarLogout');
        const slideMultishopList = document.getElementById('slideMultishopList');

        // 앱에서 접근했을 때 로그아웃, 언어 숨김 처리
        if(saved_mall_connect_type === 'app' && logoutButton && slideMultishopList) {
            logoutButton.style.display = 'none';
            slideMultishopList.style.display = 'none';
        }
    });
</script>
```

`DOMContentLoaded` 대신 `CustomEvent`인 `MallConnectTypeSet`이 발생했을 때 필요한 로직을 처리하여 좀 더 명확한 연결이 가능했다. 좀 더 리팩토링할 수 있겠지만 간소한 코드고, 해당 모듈에서는 추가적인 스크립트가 발생할 것 같진 않아 직관적으로 작성했다.

### 다른 사람은 어떻게 썼을까

글 서두에 언급했듯 다른 발표자분의 자료에서 `CustomEvent` 적용 사례를 접했었는데, 도통 기억이 나질 않아 링크드인으로 발표 자료를 요청드렸다. (감사합니다!)

[CustomEvent가 선언된 페이지](https://docs.google.com/presentation/d/1R133V2u7KFqYleClptrTkOfsf19VvvJmkxl78k1KBVM/edit#slide=id.g279e72b34fe_0_2)를 보니 마우스 `hover`나 `leave`에 따라 별도의 `CustomEvent`를 선언하고 `dispatchEvent` 한 후에 해당 이벤트에 따라 `useState`의 값을 변환하는 과정이었다. 단편적으로 자료만 봤을 땐 `CustomEvent` 대신 이미 기존에 존재하는 마우스 관련 `Event`를 활용할 수 있지 않을까,하는 의문이 들기도 했는데 이건 발표자님에게 다시 질문드려볼까 싶다. 

### 결론

카페24의 레거시 코드와 웹 표준 경시, CSS 오버라이딩 등이 적용된 스킨 제작사의 코드를 커스터마이징하는 건 괴로운 일이었지만, `CustomEvent`를 활용한 경험은 값진 일이었다.

실질적으로 많이 사용할 일은 없겠지만 React 컴포넌트 간에도 별도의 `CustomEvent`를 캐치한다면 특정 케이스에서는 유용할 수도 있다는 생각이 든다. 다만 이벤트를 과도하게 붙이면 성능 및 유지보수 측면에서 딱히 좋다고 보기 어렵고, `dispatch`하는 컴포넌트와 이를 추가하는 컴포넌트 간의 관계를 명확하게 인지하지 못하는 경우 추적 및 디버깅 자체가 어려워질 수 있다. 별도 문서로 관리하는 등의 유지보수 측면을 고려해서 사용하는 편이 좋다는 생각이 든다. 그래도 [다양한 사용 예시가 언급된 이 포스팅](https://developer-talk.tistory.com/887)을 보면 이런 케이스에서 사용할 수도 있겠구나 싶은 생각이 들었다.

추가로 [Event와 CustomEvent를 비교한 글](https://stackoverflow.com/questions/40794580/event-vs-customevent)을 보면 `CustomEvent`의 `detail` property는 재할당이 되지 않기 때문에 해당 property를 사용하는 경우 이 부분 또한 체크해보는 편이 좋을 것 같다.

### 참고자료

- https://ko.javascript.info/dispatch-events
- https://developer.mozilla.org/ko/docs/Web/API/CustomEvent/CustomEvent
- https://docs.google.com/presentation/d/1R133V2u7KFqYleClptrTkOfsf19VvvJmkxl78k1KBVM
- https://stackoverflow.com/questions/40794580/event-vs-customevent