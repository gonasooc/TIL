## 브라우저 렌더링 개요

### 렌더링 흐름

- 브라우저는 사용자의 요청에 따라 서버로부터 HTML을 포함한 에셋을 응답 받음 → HTML, CSS 파일은 렌더링 엔진의 HTML 파서와 CSS 파서에 의해 파싱(Parsing)되어 DOM, CSSOM 트리로 변환되고, 렌더 트리로 결합 → 이렇게 생성된 렌더 트리를 기반으로 레이어 합성 단계를 거쳐 웹페이지를 출력

### 자바스크립트 처리

- 자바스크립트는 렌더링 엔진이 아닌 자바스크립트 엔진이 처리 → HTML 파서는 script 태그를 만나면 자바스크립트 코드를 실행하기 위해서 진행 중이던 DOM 생성 프로세스를 중지하고 자바스크립트 엔진으로 제어 권한 위임 → 자바스크립트 실행이 완료되면 다시 HTML 파서로 제어 권한을 넘겨서 브라우저가 중지했던 시점부터 다시 DOM 생성을 재개

### script 태그의 위치

- 이처럼 브라우저는 동기(Synchronous)적으로 HTML, CSS, JavaScript를 처리하기 때문에 script 태그의 위치에 따라 블로킹이 발생하여 DOM 생성 자체를 지연시킬 수도 있음 → 때에 따라 script 태그의 위치가 중요하게 작용
- body 요소 아래에 자바스크립트를 위치하면 HTML 요소들이 스크립트 로딩 지연으로 인해 렌더링에 지장 받는 일이 발생하지 않고, DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생할 수 있는데 그런 부분을 미연에 방지할 수 있음 → 다만 자바스크립트 의존성이 높은 경우도 있어서 설계에 따라 다르게 적용할 필요성이 있음

## 웹 브라우저의 구조

- 사용자 인터페이스(User Interface)
  - 사용자와 상호작용하는 유저 인터페이스
  - URI를 입력 가능한 주소 표시 줄, 이전 버튼과 다음 버튼, 북마크, 새로 고침 버튼과 현재 문서의 로드를 중단할 수 있는 정지 버튼, 홈 버튼
  - 표준 명세가 없음에도 불구하고 수 년간 서로의 장점을 모방하면서 현재에 이름
- **렌더링 엔진(Rendering Engine) - HTML과 CSS를 파싱하여 요청한 웹 페이지를 표시하는 렌더링 엔진 → 이 과정이 Critical Rendering Path**
- 브라우저 엔진(Browser Engine) - 사용자 인터페이스와 렌더링 엔진을 연결하고 동작 제어
- 통신(Networking) - HTTP 요청과 같은 네트워크 호출에 사용, 각종 네트워크 요청을 수행하는 네트워킹 파트
- UI 백엔드(UI Backend) - 버튼이나 체크박스 같은 기본적인 장치를 그려줌, 플랫폼에서 명시하지 않은 일반적인 인터페이스로서, OS 사용자 인터페이스 체계를 사용하기에 OS에 따라 달라짐
- 자료 저장소(Data storage) - localStorage나 Cookie와 같이 보조 기억장치에 데이터를 저장하는 파트
- 자바스크립트 해석기(Javascript Interpreter) - 자바스크립트 코드를 실행하는 인터프리터(크롬의 경우 V8 엔진)
  - 자바스크립트는 렌더링 엔진이 아닌 자바스크립트 엔진이 처리

## 렌더링 엔진

- Webkit - Safari에서 사용
- Gecko - Firefox에서 사용
- Blink - Chrome에서 사용, 이전에는 Webkit을 사용했다가 2013년부터 Webkit에서 갈라져 나온 Blink 사용
- EdgeHTML - Edge 초기 버전에서 사용했으나 2018년 크로미움 기반으로 변경, 2021년 지원 종료

## 렌더링 흐름(Critical Rendering Path)

- 우리가 브라우저에 URI을 입력해서 요청하게 되면 서버는 약속되어 있는 HTML 파일을 응답
  - 주소창에 URL을 입력하면 URL의 호스트 이름이 DNS를 통해 IP 주소로 변환되고 해당 IP 주소를 소유한 서버에 요청 전송
- 이때 전송받는 HTML 코드는 8비트의 데이터 형태로 전송되게 되는데, 이런 과정이 바이트 스트림
- 바이트 → 문자로 변환 → 토큰화(토큰 검증) → 노드 생성 → **DOM 트리 생성**
  - 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽고 실행하기 위해 텍스트 문자열을 토큰으로 분해(토큰화) → 토큰에 문법적 의미와 구조를 반영해서 트리 구조의 자료 구조인 파스 트리를 생성
  - 노드 생성 시 개별 `text` 뿐만 아니라 `attribute`, `src`, `alt` 등도 개별 노드로 생성, DOM 트리 생성 중에 `img`, `link` 태그를 만나면 해당 요소가 가지고 있는 에셋 다운로드
- CSS도 CSS 파서를 통해 동일한 과정을 거쳐서 **CSSOM 트리 생성**
- DOM 트리와 CSSOM 트리를 합쳐서 **렌더 트리 생성**
  - 렌더 트리(Render Tree): 화면에 표시되어야 할 모든 노드의 컨텐츠, 스타일 정보를 포함하는 트리
  - `meta tag`, `display: none`은 포함되지 않음 → 웹 접근성 고려할 필요성 있음
- **레이아웃(Layout) 단계(reflow)** - 렌더 트리(Render Tree) 배치
  - `position`, `left`, `top`, `width`, `height`, `font-size`, `border-width` 등
- **페인트(Paint) 단계(repaint)** - 렌더 트리(Render Tree) 그리기, 모든 시각적인 요소가 픽셀 단위로 그려지는 단계
  - `background`, `box-shadow`, `border-radius`, `color` 등
- 마지막으로 **레이어 합성(Composite Layers) 단계**를 통해 브라우저는 웹페이지 출력

## CSS를 통한 브라우저 렌더링 최적화

### 최적화하는 이유

- **브라우저가 중간에 리플로우와 리페인트 작업을 다시 하는 건 부담스러운 작업**
- 리플로우 - 리페인트 - 레이어 합성이 반드시 순차적으로 실행되는 것은 아님, 각각 변경사항에 따라 각각 실행하고 최종 레이어 합성(Composite Layers) 단계로 진행
  - 각 css property가 렌더링 엔진에 따라 어떤 단계를 실행하는지 확인 가능 → [https://csstriggers.com](https://csstriggers.com/)
- **`transform`, `opacity`는 리플로우, 리페인트 생략**(GPU가 관여할 수 있는 속성 - DOM 트리를 변경하지 않도록 설계되어 있음 - 즉, 브라우저가 상대적으로 쾌적하게 렌더링할 수 있음)

### 최적화 예시

- `left`, `right`, `top`, `bottom` - 리플로우, 리페인트가 필요 → **`trasform: translate(x, y)` - 리플로우, 리페인트 생략, GPU 가속 활용**
- `trasform: translate(x, y)` - 레이어 최적화 없음 → **`trasform: translate3d(x, y, z)` - 레이어 최적화 진행(브라우저는 해당 요소를 그래픽 레이어로 처리)**

### Graphics Layer(↔ Paint Layer)

- 그래픽 레이어는 GPU를 활용하여 이미지로 변환되며 병렬처리에 특화되어 있어 더 빠르게 변환
- 기존 레이어에서 새롭게 분리되어 주변 레이어에 영향 없이 해당 레이어만 빠르게 그리고, 출력할 수 있음
- GPU에서 작업이 이뤄져 CPU와 그래픽 작업을 분리할 수 있어 저사양 기기에서의 CPU 부담을 줄일 수 있음

## 참고자료

- https://poiemaweb.com/js-browser
- https://youtu.be/z1Jj7Xg-TkU?si=3gpkjCRefWxQj8Vx
- https://youtu.be/sJ14cWjrNis?si=eJ3Iqnwpu9k9dZAM
- https://youtu.be/TZz9VHjJzMk?si=grc9Uz3F_2zsz-lQ
- https://d2.naver.com/helloworld/2061385
- [https://csstriggers.com](https://csstriggers.com/)
