# 2곳 중 1곳은 무조건 합격하는 개발자 이력서 만들기 - 지소라

## 피드백 그 무한굴레 - 피드백 그 끝은 어디인가

- “네 이력서는 너무 말랑말랑해” → 이력서 = 가장 dry한 문서 → T의 문서
  - ex) 애플리케이션 UX개선(Why)을 위해 Vue.js를 이용한(How) 유료회원 전용 웹 애플리케이션 기획, UI 디자인 설계 및 테크 리드(What)
- “길다” → “컴팩트하게 만드는 것도 능력” → 메인 내용은 1장 내 권유
- “그래서 뭘 할 줄 안다는 거에요?” → “이런 것도 해봤어요”식의 뱃지 모으기보다는 “이렇게 해본 것으로 이런 것까지 만들어 봤어요”가 중요
- “내용이 개발에 초점이 맞춰져 있지 않다” → 기획 능력(X) → 개발자 = 기술적으로 문제를 해결하는 사람 → 어떤 문제가 있었고, 어떤 식으로 구체화했고, 기술적으로 어떻게 해결했는지

## NOT ENOUGH, I NEED A KICK - 포트폴리오를 킥으로 사용하자

- “여태까지의 작업물을 모아볼 수 있는 모음집 부재”
- “얼마만큼의, 어떤 고민을 가지고 기술적으로 풀어냈는지 알 수 없음”
- “Problem Solving의 스토리를 작성” → “서사(=스토리)를 작성”
- “주인공인 **우리**가 어떠한 **외적 문제**를 마주한 뒤 **무엇을** 위해서 **어떻게** 문제 해결을 했는지”
- 포트폴리오 작성 순서
  - AS-IS
    - 문제 상황 인지
    - 해결하려고 하는 문제
    - 만들고 싶은 기능
  - Challenge
    - 문제해결을 위해 고민한 내용
    - 어떻게 기술적으로 해결했는지
  - TO-BE
    - 아웃풋(결과)

# 타입스크립트는 왜 그럴까?: 집합으로 이해하는 타입스크립트 - 이정환

## 타입을 집합으로 이해하기

- String 타입, Number 타입 …
- Number 타입 > Number 리터럴 타입 → 슈퍼 타입(부모)인 Number 타입 > 서브 타입(자식)인 Number 리터럴 타입
- 타입 계층도로 이해하는 슈퍼 타입과 서브 타입

## 타입 호환성 이해하기

- 특정 타입의 값을 다른 타입으로 취급해도 괜찮은지 판단하는 것
- 슈퍼 타입과 서브 타입의 관계 속에서 업 캐스팅과 다운 캐스팅 체크

## 타입 계층도와 함께 특수한 타입들 이해하기

- Unknown → 모든 타입의 슈퍼 타입 → 전체 집합
  - 모든 타입의 값을 저장할 수 있음 → 업 캐스팅에 해당하는 연산이기 때문
  - 어떤 타입의 변수에도 저장할 수 없음 → 다운 캐스팅에 해당하는 연산이기 때문
  - 현재 정확한 타입을 알기 어려울 때 사용 → 타입 좁하기(narrowing)와 함께 값을 유연하게 사용 가능
- Never → 모든 타입의 서브 타입 → 공집합
  - 모든 타입의 변수에 저장 가능 → 업 캐스팅에 해당하는 연산이기 때문
  - 어떤 타입의 값도 저장할 수 없음 → 다운 캐스팅에 해당하는 연산이기 때문
  - 호출되지 않아야 하는 함수를 만들 때, 정상적으로는 도달할 수 없는 함수를 만들 때(ex) error 함수), switch문의 완전성을 보장하기 위해 활용 가능
- Any → 타입 계층도를 무시 → 치트키

  - 타입 검사를 받지 않음
  - 예외적으로 Never 타입의 변수에는 저장 불가
  - 불가능한 타입 단언을 가능케 할 수 있음

    ```tsx
    // Not Works
    let str1: string = 10 as string;

    // Works
    let str2: string = 10 as any as string;
    let str3: string = 10 as unknown as string;
    ```

## 객체 타입을 집합으로 바라보기

- 구조적 타입 시스템

  - 프로퍼티를 기준으로 타입을 정의함

    ```tsx
    let person: Person = {
      name: "이정환",
    };

    let student: Student = {
      name: "이정환",
      school: "가톨릭대",
    };

    // Works - 업 캐스팅
    person = student;

    // Not Works - 다운 캐스팅
    student = person;
    ```

## 대수 타입을 집합으로 바라보기

- 대수 타입이란? → 둘 이상의 타입을 합쳐 만든 타입 → Union, Intersection

# Turborepo, Next.js, TypeScript를 이용한 프론트엔드 모노레포 적용기 - 김우현

## 모노레포란?

- 하나의 레포지토리에서 독립적인 여러 프로젝트를 관리하는 방법
- 기존에 많이 쓰던 Multi Repo
  - ex) App, Design System, Lib_A, Lib_B 등의 개별 프로젝트 단위가 각각의 레포에서 관리 → 이걸 하나의 단일 레포에서 관리하는 걸 모노 레포
- 장점
  - 빠른 코드 수정
  - 각 레포마다 사용했던 같은 코드들의 중복 제거
  - 수월한 코드 리팩토링
  - 코드 컨벤션 통일
  - 통합 CI, test 관리
- 단점
  - 의존성 관리 복잡 - 과도한 의존 관계가 생길 수 있음
  - 무거운 프로젝트 (CI 속도 저하)
  - Code ownership 위배

## 모노레포를 선택하게 된 계기?

- 개발자 시점 / 라이브러리 사용하는 개발자 시점 / 운영팀 시점 등.. 여러 시점이 존재하는 경우

## 왜 Turborepo 인가?

- https://d2.naver.com/helloworld/7553804

## Turborepo 적용하기

- Next.js / TypeScript / Yarn 사용 (공식에서는 pnpm 추천)

## 적용 후 장/단점

- 장점
  - 패키지 분리로 인해 공통 로직 관리가 쉬워졌다
  - 코드 구조가 한눈에 잘 보인다
  - Next, TypeScript와 찰떡콩떡
- 단점
  - 의존성 관리가 힘들다
  - 패키지 간의 의존 관계 설계 복잡
  - 모든 패키지를 테스트 하기 때문에 CI는 역시 오래 걸린다

## 결론

- 모노레포는 하나의 제품에 필요한 패키지를 여러 개 만들어야 할 때 적절한 대안

# 실시간 추천 시스템 구축하기 - 정지용

## Session-based Recommender

- Session-based Recommender
- 가장 중요한 User Feature?
  - 유저가 콘텐츠를 소비한 ‘체류시간’
- ex) 예시 데이터
  ```tsx
  {
  	...
  	'action_type': 'view',
  	'user_id': 4040441,
  	'resource_type': 'article',
  	'resource_id': 23411230,
  	'timestamp': 1684821964,
  	...
  }
  ```
- 이런 식으로 발생된 유저 로그 → Apache Beam을 통해 Data Processing → BigTable에 저장
- BIgTable에 유저 로그는 user_id를 key값으로 해서 resource_id, session_id, timestamp 등이 value로 담기게 됨
- timestamp 기반으로 체류 시간을 계산

# SSR의 기쁨과 슬픔: 렌더링의 변화의 흐름을 통해 알아보는 SSR과 Streaming SSR - 김태희

### Static Web Site

- 웹 서버에서 정적인 HTML 파일을 중심으로 제공
- URL을 통해 해당 디렉토리에 있는 파일을 내려주는 방식
  - 디렉토리와 파일 구조가 곧 URL
  - 해당 경로 폴더에 각각의 index.html이 있고 요청을 주고 받는..

### Common Gateway Interface

- **정적인 HTML 제공의 한계를 탈피하기 위해 등장**
- Apache, NGINX 등을 통해 가능
- 웹서버에 요청이 들어오면 외부 프로그램을 호출하여 그것을 처리한 결과를 내려줌

### Server Side Template

- Web Server에서 정적인 HTML을 제공하는 게 아닌, Server Application에서 여러 가지 조건과 로직에 따라 HTML을 생성해서 내려주는 방식
- Server Application은 Java, Python 등 다양한 언어를 통해 제공
  - 하나의 언어에서도 굉장히 다양한 Template이 존재
- HTML은 Server에서 내려주고 브라우저에서 Rendering하지만, 브라우저에 Rendering 된 이후 인터렉션 처리 등은 JavaScript를 Client에서 실행하여 처리
- ex) JSP, handlebars, Thymeleaf, Jinja, EJS..

### Server Side Template 문제점

- UI가 복잡하지 않던 시절에는 이것으로 충분
- 기능 구현에 따라 같은 뷰 로직이 Server side에서도 구현되어야 했고 Client에서도 구현이 되어야 했음
  - ajax를 통해 데이터를 불러와 Client에서 랜더링은 추가로 하는 경우 때문
    - 더보기 기능이나 무한스크롤 등
  - 결국 이렇게 랜더링 시점이 뒤섞이게 되면서 복잡한 인터렉션의 UI를 다룰수록 작업자의 고통은 점점 커짐
- Server에서 API나 Database Query 등에서 시간이 오래 걸릴 경우, 사용자에게 보여지는 화면이 늦어지는 문제가 있음
  - 이 부분 때문에 API 요청 등에서 오래 걸릴 경우, FID 점수가 낮아짐

### Client Side Rendering

- 브라우저 성능의 발전과 JS 스펙의 고도화로, 아예 Rendering을 Client에서 다 처리해버리자는 움직임이 등장
- API와 웹 어플리케이션이 완전히 분리되어 있고, 웹 애플리케이션은 HTML, CSS, JS만 있는 형태, 현재에 익숙한 형태
- 따라서 뷰 로직은 전부 Client에 있고, Server에선 API 형태로 데이터만 내려주는 형태
- Client에는 정적인 파일들이 주를 이루기 때문에 개념적으로 Static Web Site와 거의 같음
  - 그렇기 때문에 별도의 Application Server 없이 CDN을 통한 배포
  - Application Server가 없기 때문에 신경 쓸 요소가 적음
  - 트래픽에 대응해서 스케일 업이나 스케일 아웃을 할 필요가 없음, CDN에서 알아서 해주기 때문
- 기존 Server Side 방식에서 해주던 URL에 따른 라우팅 처리도 Client에서 처리
  - 이것이 바로 SPA

### Client Side Rendering의 흐름

- 기본적으로 404 에러가 발생하면 index.html로 요청을 돌림

### Client Side Rendering - 문제점

- SPA 형태는 어느 페이지를 접속해도 매번 같은 index.html를 내려주기 때문에 meta:og 태그 관련 문제 발생
- 그렇기에 크롤러/봇 입장에서는 Client가 HTML을 그리기 전 HTML만 받아올 수 있음
  - AWS의 경우 lambda edge, firebase의 경우 functions 등의 서비스를 통해 보완은 가능하나 추천할 만한 방법은 아님
- 초기에 body가 텅 비어 있다가 채워지는 방식이라 초기 랜더링이 느림
- 메모리 누수에 좀 더 취약함 → SPA는 별도의 새로고침 없이 계속 화면을 다시 그리기 때문에 새로고침을 통한 메모리 누수 관리가 상대적으로 취약함

### Server Side Rendering

- node.js의 발전으로 Client와 Server에서 같은 언어를 쓸 수 있게 되면서 다시 Rendering의 책임을 Server로 돌리려는 움직임이 나옴
- 같은 언어로 양쪽 다 쓸 수 있음
- Server Side Template 기반의 Rendering과 유사한 측면이 있음

### Server Side Rendering의 단점

- 더 이상 serverless가 아니게 되므로 server 관리 책임이 생김
- 관리해야 하는 server가 있기 때문에 트래픽이 몰릴 경우 스케일 업, 스케일 아웃 등의 작업이 필요해짐
- docker 등으로 말아서 배포할 경우 빌드 및 배포 시간이 CSR 대비해서 더 늘어남
- 세팅이 복잡함
  - next,js, remix, vite-plugin-ssr 등이 등장하면서 많이 보완된 단점
- CSR 경험만 있던 사람들에게는 다소 개념이 복잡하게 느껴질 수 있음

### 당근마켓 동네생활 이야기

- 웹뷰(기반 클라이언트)를 다양한 서비스에서 활용
- 대부분의 서비스가 React 기반의 CSR 기반
  - 배포는 PAAS(platform as a service)나 Serverless Hosting 방식
    - 대표적으로는 vercel이나 cloudflare
- 이 웹뷰를 초기에 로딩하는 걸리는 시간이 생각보다 오래 걸림
  - 아마 CSR에 필요한 자원들을 초기에 받고 ,실행하는 데 걸리는 시간이 꽤 걸림
  - 특히 배포 직후에는 JS Bundle이나 CSS 등을 새로 받아야 하는데 생각보다 로딩이 걸림
- 기존의 네이티브 앱을 웹뷰로 전환하는 의사 결정
  - 사용자 측면에서 화면을 초대한 빨리 뜨게 하는 것을 고민하게 됨
  - 개인적으로 next.js + ISR 기반으로 정적 생성을 선호하지만, 규모 측면에서 해결해야 하는 문제가 있었음
  - 결국 SSR로 시도
- 당근마켓에서는 stackflow라는 액티비티 관리 프레임워크(웹뷰를 네이티브처럼 보이게 하는)가 있고 활용중

### 구현하고 보니…

- @tankstack/react-query를 사용해서 데이터를 가져오게 구현을 했었음
- 습관처럼 Suspense를 통해 로딩 처리를 했음
- 그런데, ReactDOM.renderToString은 suspense를 지원하지 않음
- 그러니까 사실상 SSR이 완전하게 되진 않음
  - SSR이 되다가 Suspense 처리가 되는 구간을 만나면 CSR로 전환이 되어버림
- 그래서 일단 Server Side에서는 prefetch 처리만 먼저 했음
  - 이 정도만 해도 속도 체감이 됨

### 기쁨도 잠시, SSR의 슬픔이 시작되고…

- 기존 vecel 배포 대비, 인프라 관리와 서버 운영에 대해 너무나도 신경을 많이 써야 함
  - 기존에는 볼 필요 없었던 Node.js Server 모니터링도 해야 하고, Node.js Server의 로그도 봐야 함
- docker 기반의 배포를 하기 대문에 vercel, cloudflare 대비 배포 시간이 길어짐
- 특정 상황에서 랜더링이 느려지는 문제

### Streaming SSR

- 기존 SSR의 단점을 보완하는 방법
- 기존에는 Server에서 HTML을 전부 생성한 이후 화면을 그리는 방식
- Streaming SSR의 경우는 가장 빠르게 그릴 수 있는 부분을 먼저 그리고, 이후는 Node.js의 스트림을 이용해 점진적으로 랜더링하는 방식
  - renderToPipteableStream 함수를 이용하면, Node.js 스트림을 이용한 방식으로 랜더링이 가능
- https://mxstbr.com/thoughts/streaming-ssr/
- Streaming SSR에서는 Shell, 그리고 Suspense로 경계 지어진 컴포넌트를 기준으로 렌더링을 처리함
- Suspense로 감싸진 최상단 영역 밖의 영역을 Shell이라고 함
  - Shell은 일반적으로 GNB 등 data fetching이 필요없는 부분이라고 볼 수 있음
- Shell이 먼저 렌더링 된 후, 이후 Suspense로 나눈 부분을 Streaming 하는 방식
