# 라운드테이블 : 토스 시니어 개발자가 말하는 커리어 패스

[토스ㅣSLASH 23 - 라운드테이블 : 토스 시니어 개발자가 말하는 커리어 패스](https://youtu.be/Y96tr9SVNy8)

**#시니어 개발자도, PO도 아니었기 때문에 가볍게 보기 좋았음, 기술적으로 잘 성장한다면 나중에 TPO로 직무 전환을 하는 것도 즐거운 일이 될 수도 있겠다 싶음**

## 흔치 않은 직군, TPO로 일하는 것의 의미

- 기존의 PO, PM 직군에 있는 분들은 주로 기획자, 비개발자 출신이 많다 보니 그 부분에서 TPO는 남들이 하기 어려운 일들을 할 수 있는 포지션

## 개발자에서 PO로, 전직을 결심한 계기

- 개발자로서의 성장이 많이 더뎌졌다는 생각이 가장 컸음
- 주변의 PO분들을 보면서 나도 저분들이 잘하는 영역을 해보고 싶다는 느낌이 컸음
- PO를 하는 사람에게 기술 공부를 해서 TPO가 되어라 하면 잘 안 되기 때문에 시니어 개발자에게 기획적 공부를 권하는 게 빠르다 (…!)

## 개발자 이후의 커리어를 고민하는 분들에게 한마디

- 창업에 관심 있다면 TPO에 도전해보는 것도 좋은 기회

# 달리는 토스 앱에 React Native 엔진 더하기

[토스ㅣSLASH 23 - 달리는 토스 앱에 React Native 엔진 더하기](https://youtu.be/6H9WQDRFZYg)

**#어떤 변화를 가져가는 시점에서 문제를 명확히 도출하고 솔루션에 접근해가는 과정에 인상적, 꼭 개발뿐만 아니라 모든 문제에 있어서 순차적 접근이 중요하다는 생각이 문득 듬**

## 왜 React Native인가?

### 토스 프론트엔드 챕터의 중요한 가치

- 사용자 경험, 개발자 경험

### 사용자 경험 Web vs. React Native

- 로딩 시간 100명 중 5명은 3.5초 vs. 0.X초
- 웹개발 환경은 완전히 화면을 그리기 위해 HTML과 JS를 내려 받아야 하기 때문에 네트워크 성능에 많이 의존함 / 반면 RN은 이미 파일 시스템이 저장되어 있는 JS 파일을 참조하기 때문에 네트워크 성능과 무관하게 첫 화면을 완전하게 그릴 수 있음
- 개발자 경험: 서비스 빌드 속도 차이
- 개발자 경험: SSR 서버 운영 비용, CDN 운영 비용

## React Native 도입 과정에서 기술적 도전

### 서비스별 배포

- Shared와 Service의 파일 분리

### 빌드 속도 감축

- Metro의 느린 빌드 속도 때문에 esbuild로 교체

### 8개 언어, 3개 플랫폼

- React Native Test Page를 통해 작업

### 의존성 관리

- Yarn Plu’n’Play 도입

# 누구나 쓸 수 있는 접근성 높은 토스 만들기

[토스ㅣSLASH 23 - 누구나 쓸 수 있는 접근성 높은 토스 만들기](https://youtu.be/_GwTZnhLcDI)

**#순차적으로 접근성에 대응하고 있다는 부분이 인상적 → 특히 큰 글씨의 경우, 웹개발을 할 때 이미 말줄임이 적용되어 있는 경우 디바이스에 따라 정보의 양 차별이 있을 수 있다는 점은 인지했으나 그게 알 권리 침해라고까진 생각치 못했음, 접근성은 놓칠 수 있기 때문에 한번 정도 생각하고 않하고,가 중요하다는 생각** 

## 모바일 접근성을 위한 TDS 접근성 대응

### 큰 글씨 모드

- maxLIne을 한 줄로 강제하고 말줄임 표시를 한 상태에선 단순히 글씨를 키우는 것으론 해결할 수 없음 → 개발 난이도가 낮고 UI 구성이 일관적으로 유지되지만 사용자의 알 권리 침해 → 사용자의 환경에 따라 정보의 양 차별이 있어서는 안 된다는 토스의 규칙이 지켜지지 못함
- 모든 컴포넌트에서 maxLine 해제 → 단순히 말줄임에서 컴포넌트 위치를 다변적으로 변경
- 한정된 탭 영역 내에서 탭 내용을 보여줘야 하는 레이아웃 변경 배치만으로는 해결하지 못하는 경우는 탭을 누르면 탭의 내용 전체를 보여주는 BottomSheet로 해결

### 개행문자

- 글씨를 키웠을 때 개행 레이아웃이 어그러지는 부분에선 개행을 공백으로 대체
    
    ```jsx
    text.place("\n", " ");
    ```
    

### 스크린 리더 기능

- 대체 텍스트와 초점 개선으로 인한 스크린 리더 대응

### 상태 정보 갱신

- 스크린 리더는 기본적으로 화면 정보가 변할 때 자동으로 읽어주지 않음
- 상태가 변경될 때마다 변경된 상태를 읽어주도록 기본 동작을 변경

# 미친 생산성을 위한 React Native

[토스ㅣSLASH 22 - 미친 생산성을 위한 React Native](https://youtu.be/b_6CjuvVg8o)

**#앞서 봤던 ‘달리는 토스 앱에 React Native 엔진 더하기’의 RN 도입 과정을 좀 더 자세하게 다루고, 회사 차원에서 RN을 어떤 회사가 도입하면 좋을지에 대한 내용들, 별도의 메모보다는 조금 빠르게 스킵하면서 봤음, 우직하게 플래닝하고 순차적으로 실행해나가는 과정이 인상적**

# 퍼널: 쏟아지는 페이지 한 방에 관리하기

[토스ㅣSLASH 23 - 퍼널: 쏟아지는 페이지 한 방에 관리하기](https://youtu.be/NwLWX2RNVcw)

**#퍼널이라는 용어를 처음 알게 됐고, 흔한 페이지 흐름에서는 비슷한 고민을 하는구나 싶었음, 구조를 컴포넌트화한 게 Vue2 v-if 구조와 비슷해서 반가웠음ㅋ**

[https://slash.page/ko/libraries/react/use-funnel/readme.i18n/#퍼널-정의하기](https://slash.page/ko/libraries/react/use-funnel/readme.i18n/#%ED%8D%BC%EB%84%90-%EC%A0%95%EC%9D%98%ED%95%98%EA%B8%B0)