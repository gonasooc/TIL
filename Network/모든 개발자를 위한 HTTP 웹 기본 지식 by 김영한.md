### 인터넷 통신

- 수많은 노드를 거쳐 클라이언트와 서버가 통신

### IP(인터넷 프로토콜)

- 노드를 통해 통신하기 위해 IP가 필요
- 패킷(Packet)이라는 통신 단위로 데이터 전달
- IP 프로토콜의 한계
    - 비연결성 - 패킷을 받을 대상이 없거나 서비스 불능 상태에도 패킷 전송
    - 비신뢰성 - 중간에 패킷이 사라지면?, 패킷이 순서대로 안오면?
    - 프로그램 구분 - 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?
    - 패킷 전달 순서 문제 발생 - e.g., 연관성 있는 두 개의 패킷에서 1번 패킷보다 2번 패킷이 먼저 도착할 수 있음
- 이런 인터넷 프로토콜의 한계를 TCP/UDP를 통해 해결

### TCP/UDP

- e.g., 특정 메세지를 SOCKET 라이브러리를 통해 전달, TCP 정보 생성-메세지 데이터 포함, IP 패킷 생성-TCP 데이터 포함
- 전송 제어 프로토콜
    - 연결지향 - TCP 3 way handshake (실제 연결이 아닌 가상의 연결 개념)
    - 데이터 전달 보증
    - 순서 보장
    - 신뢰할 수 있는 프로토콜
- TCP 3 way handshake - SYN: 접속 요청, ACK: 요청 수락, ACK와 함께 데이터 전송 가능
    1. SYN - 클라이언트가 서버에게 보냄
    2. SYN+ACK - 서버가 클라이언트에게 보냄
    3. ACK - 클라이언트가 서버에게 보냄
- 데이터 전달 보증 - 전송과 잘 받았다는 응답을 하기 때문에 전달 보증
- 순서 보장 - 패킷이 순서대로 도착하지 않으면 클라이언트에게 재요청
- TCP에는 전송 제어, 순서, 검증 정보 등이 포함되어 있기 때문에 가능
- UDP
    - 기능이 거의 없음, IP와 거의 같고 PORT와 체크섬 정보만 추가
    - 하나의 클라이언트에게 여러 개의 애플리케이션 작업이 필요할 때 PORT 부여
    - 최적화를 위해선 UDP를 통해 설정
    - 최근에 3 way handshake을 줄이는 등의 최적화의 일환으로 UDP를 사용하기도 함

### PORT

- 같은 IP 내에서 PORT로 구분
- 0 ~ 65535 할당 가능
- 0 ~ 1023: 잘 알려진 포트, 사용하지 않은 편이 좋음
    - FTP - 20, 21
    - TELNET - 23
    - HTTP - 80
    - HTTPS - 443

### DNS

- IP는 기억하기 어렵고 변경될 수 있음
- DNS 서버에 도메인명과 IP를 저장

### URI

- URI(Uniform Resource Identifier) - locator, name 또는 둘 다 추가로 분류될 수 있음
    - URL(Resource Locator)
        - foo://example.com:8042/over/there?name=ferret#nose
        - scheme, authority, path. query, fragment
    - URN(Resource Name)
        - urn:example:animal:ferret:nose
        - 책의 isbn URN - urn:isbn:8960777331
- URL - scheme://[userinfo@]host[:port]/[/path][?query][#fragment]
    - 일반적으로 생략, http는 80, https는 443
        
        fragment - HTTP 문서 내부에서 특정 위치로 가거나 하이라이트하는 등에 사용
        

### 웹 브라우저 요청 흐름

- 클라이언트가 요청 패킷 전달
    - DNS 서버 조회 및 포트 정보 체크
    - 웹 브라우저가 HTTP 메시지 생성(GET)
    
    (TCP 3 way handshake를 통해 가상 연결로 확인)
    
    - SOCKET 라이브러리를 통해 전달
        - TCP/IP 연결(IP, PORT)
        - 데이터 전달
    - TCP/IP 패킷 생성, HTTP 메시지 포함
    - 네트워크 인터페이스(LAN 드라이버, LAN 장비)를 통해 서버로 전달(HTTP 요청 메시지를 보냄)
- 서버가 요청 패킷 도착
    - TCP/IP 패킷은 까서 버리고 HTTP 메시지를 해석
    - HTTP 응답 메시지를 포함한 패킷을 동일하게 만들어서 전달
- 클라이언트 웹 브라우저가 HTML 렌더링

### 모든 것이 HTTP

- HTTP/1.1 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전
- 기반 프로토콜
    - TCP: HTTP/1.1, HTTP/2
    - UDP: HTTP/3
    - 현재 HTTP/1.1 주로 사용
        - HTTP/2, HTTP/3도 점점 증가 추세
- HTTP 특징
    - 클라이언트 서버 구조
    - 무상태 프로토콜(스테이스리스), 비연결성
    - HTTP 메시지
    - 단순함, 확장 가능

### 클라이언트 서버 구조

- Request Response 구조

### 무상태 프로토콜 - 스테이스리스(Stateless)

- 상태 유지 - Stateful
    - 서버가 클라이언트 이전 상태를 보존
    - 항상 같은 서버가 유지되어야 함
- 무상태 - Stateless
    - 갑자기 클라이언트 요청에 증가해도 서버를 대거 투입 가능, 무상태는 응답 서버를 쉽게 바꿀 수 있음 → 무한한 서버 증설 가능
    - 스케일 아웃 - 수평 확장 유리
- Stateless 실무 한계
    - 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
    - 상태 유지는 최소한만 사용

### 비 연결성(connectionless)

- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위 이하의 빠른 속도로 응답, 웹 브라우저 환경에서는 연속해서 데이터를 호출하는 경우가 적음
- 한계와 극복
    - TCP/IP 연결을 새로 맺어야 함 - (사용자 입장에선) 3 way handshake 시간 추가
    - 요청하면 수많은 자원이 함께 다운로드
    - 현재는 HTTP 지속 연결(Persistent Connections)로 문제 해결
    - HTTP/2, HTTP/3에서 더 많은 최적화
- HTTP 초기 - 연결, 종료 낭비
    - HTML, 자바스크립트, 이미지 등 각각의 요청에 마다 응답을 받고 종료하는 낭비
- HTTP 지속 연결(Persistent Connections)
    - 최초 연결 후 HTML, 자바스크립트, 이미지 응답이 다 끝날 때까지 지속 연결
- 스테이스리스를 기억하자 - 서버 개발자들이 어려워하는 업무
    - 같은 시간에 맞춰서 발생하는 대용량 트래픽

### HTTP 메시지

- HTTP 메시지 구조
    - start-line 시작 라인
    - header 헤더
    - empty line 공백 라인 (CRLF)
    - message body
- 시작 라인 - 요청 메시지
    - start-line = **request-line** / status-line
    - request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
    - HTTP 메서드
        - 종류: GET, POST, PUT, DELETE
    - 요청 대상 - absolute-path[?query] e.g., /search?q=hello&hl=ko
    - HTTP Version
- 시작 라인 - 응답 메시지
    - start-line = request-line / **status-line**
    - status-line = HTTP-version SP status-code SP reason-phrase CRLF
    - HTTP 버전
    - HTTP 상태 코드
        - 200: 성공
        - 400: 클라이언트 요청 오류
        - 500: 서버 내부 오류
    - 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글
- HTTP 헤더
    - header-field = field-name: OWS field-value OWS (OWS: optional whitespace)
    - HTTP 전송에 필요한 모든 부가정보 e.g., body의 내용, body의 크기, 압축, 인증 등..
    - 표준 헤더가 너무 많음
    - 필요 시 임의의 헤더 추가 가능
- HTTP 메시지 바디
    - 실제 전송할 데이터
    - HTML 문서, 이미지, 영상, JSON 등 byte로 표현할 수 있는 모든 데이터 전송 가능

### HTTP API를 만들어보자

- API URI 고민
    - 리소스의 의미는 뭘까? → 특정 개념 자체가 리소스
    - 리소스만 명확하게 식별 → 리소스를 URI에 매핑
- 리소스와 행위를 분리
    - URI는 리소스만 식별
    - 리소스와 해당 리소스를 대상으로 하는 행위를 분리
        - 리소스: 회원
        - 행위: 조회, 등록, 삭제, 변경
    - 리소스는 명사, 행위는 동사

### HTTP 메서드 - GET, POST

- 주요 메서드
    - GET: 리소스 조회
    - POST: 요청 데이터 처리, 주로 등록에 사용
    - PUT: 리소스를 대체, 해당 리소스가 없으면 생성
    - PATCH: 리소스의 부분 변경
    - DELETE: 리소스 삭제
- 기타 메서드
    - HEAD: GET과 동일하지만 메시지 부분을 제외하고 상태 줄과 헤더만 변환
    - OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
    - CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
    - TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행
- GET
    - 리소스 조회
    - 서버에 전달하고 싶은 데이터는 query를 통해 전달
    - 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 서버가 많아서 권장하지 않음
- POST
    - 요청 데이터 처리
    - 메시지 바디를 통해 서버로 요청 데이터 전달, 서버는 요청 데이터를 처리
    - 주로 신규 데이터 등록(e.g., 회원 추가), 프로세스(e.g., 결제완료 → 배달시작 → 배달완료) 처리에 사용
    - 다른 메서드로 처리하기 애매한 경우
        - 예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
        - 애매하면 POST
        - 단, POST는 캐싱하기 어려움, GET은 캐싱하기로 약속되어 있음

### HTTP 메서드 - PUT, PATCH, DELETE

- PUT
    - 리소스를 대체 - 있으면 대체, 없으면 생성, 쉽게 말해 덮어씌움
    - 중요! 클라이언트가 리소스를 식별 가능해야 함(특정 pk값을 path에 전달)
    - PATCH와의 차이점이라면, 기존 리소스에 값을 보내면 리소스 전체가 대체됨, 즉 username을 안 보내면 기존 username 필드는 삭제됨, 수정이 아니라 리소스를 갈아치운다고 이해해야 함
- PATCH
    - 리소스 부분 변경
    - PATCH 사용이 불가한 경우 POST 사용
- DELETE
    - 리소스 제거

### HTTP 메서드의 속성

https://ko.wikipedia.org/wiki/HTTP

- 안전(Safe) - 호출해도 리소스는 변경하지 않는다. 리소스에만 해당.
- 멱등(Idempotent) - 한번 호출해도 두번 호출하든 100번 호출하든 결과가 똑같다.
    - GET - 조회만 하기 때문에 멱등
    - PUT - 같은 body를 계속 대체하기 때문에 멱등
    - DELETE - 같은 요청을 여러 번 해도 삭제된 결과는 같기 때문에 멱등
    - POST - 중복 신청, 중복 결제가 발생하기 때문에 멱등하지 않음
    - 활용
        - 자동 복구 메커니즘
        - 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때 클라이언트가 같은 요청을 해도 되는가에 대한 판단 근거
    - 멱등은 외부 요인으로 중간에 리소스가 변경되는 것까지는 고려하지 않음
- 캐시가능(Cacheable)
    - GET, HEAD, POST, PATCH (이론상) 캐시 가능
    - 실제로는 url만 가지고 설정하면 되는 GET이나 HEAD 정도만 캐시 가능, POST, PATCH는 body 내용까지 캐시 키로 고려해야 해서 구현이 쉽지 않음

### 클라이언트에서 서버로 데이터 전송

- 쿼리 파라미터를 통한 데이터 전송
    - GET
    - 주로 정렬 필터(검색어)
- 메시지 바디를 통한 데이터 전송
    - POST, PUT, PATCH
    - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경
- 예시
    - 정적 데이터 조회 - 쿼리 파라미터 미사용
    - 동적 데이터 조회 - 쿼리 파라미터 사용
    - HTTL Form 데이터 전송
        - POST인 경우, 웹 브라우저가 알아서 Content-Type: application/x-www-form-urlencoded, body에 해당 입력 내용 삽입(key=value 형식으로)
        - GET인 경우, 웹 브라우저가 쿼리 파라미터로 붙임, 단 save는 GET을 써서 안됨
        - Content-Type: multipart/form-data로 보낸 경우 웹 브라우저가 알아서 Content-Type: multipart/form-data; boundary=——XXX 삽입, 주로 파일 업로드 같은 binary 데이터 보낼 때 사용
            - Content-Type: application/json으로 고정하는 경우 binary 데이터 보내더라도 boundary 생성이 안 돼서 전송 불가능
        - HTML Form 전송은 GET, POST만 지원

### HTTP API 설계 예시

- URI는 리소스만 식별
- 리소스와 해당 리소스를 대상으로 하는 행위를 분리
    - 리소스: 회원
    - 행위: 조회, 등록, 삭제, 변경
- 회원 관리 시스템 - POST 기반 등록
    - 회원 목록 /members → GET
    - 회원 등록 /members → POST
    - 회원 조회 /members/{id} → GET
    - 회원 수정 /members/{id} → PATCH, PUT, POST
        - 개념적으론 PATCH가 가장 적당, 다만 게시물 수정처럼 완전히 덮어 씌우는 경우는 PUT 사용 가능, 둘 다 애매하면 POST 사용 가능
    - 회원 삭제 /members/{id} → DELETE
- POST 신규 자원 등록 특징
    - 클라이언트가 등록될 리소스 URI 몰라도 등록 가능
    - 서버가 새로 등록된 리소스 URI를 생성해줌 e.g., Location: /members/100
    - 컬렉션(Collection)
        - 서버가 관리하는 리소스 디렉토리
        - 서버가 리소스의 URI를 생성하고 관리
        - 여기서 컬렉션은 /members
- API 설계 - PUT 기반 등록
    - 파일 목록 /files → GET
    - 파일 조회 /files/{filename} → GET
    - 파일 등록 /files/{filename} → PUT
    - 파일 삭제 /files/{filename} → DELETE
    - 파일 대량 등록 /files → POST
- PUT - 신규 자원 등록 특징
    - 클라이언트가 리소스 URI를 알고 있어야 등록 가능
    - 클라이언트가 직접 리소스의 URI를 지정
    - 스토어(Store)
        - 클라이언트가 관리하는 리소스 저장소
        - 클라이언트가 리소스의 URI를 알고 관리
        - 여기서 스토어는 /files
- 주로 사용하는 건 POST 기반의 등록(Collection)
- HTML FORM 사용
    - HTML FORM은 GET, POST만 지원
    - 컨트롤 URI
        - 제약 때문에 컨트롤 URI를 사용하여 동사로 된 리소스 경로 사용
- https://restfulapi.net/resource-naming/

### HTTP 상태코드 소개

- 1xx (Informational): 요청이 수신되어 처리중
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행동이 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함

### 2xx - 성공

- 200 OK - 요청 성공
- 201 Created - 요청 성공해서 새로운 리소스가 생성됨
    - 생성된 리소스는 응답의 Location 헤더 필드로 식별
- 202 Accepted - 요청이 접수되었으나 처리가 완료되지 않음(사용 빈도 낮음)
    - e.g., 요청 접수 후 1시간 뒤에 배치 프로세스가 수행
- 204 No Content - 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
    - e.g., 웹 문서 편집기에서 save 버튼

### 3xx - 리다이렉션

- 리다이렉션 이해 - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
    - e.g., GET /old-page HTTP/1.1 요청을 보내면 서버가 인지하고 301에 Location 헤더 필드를 담아서 응답 → 자동 리다이렉트 후 GET/new-page HTTP/1.1 요청, 서버에서 200 내려줌
    - 영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동
    - 일시 라디이렉션 - 일시적인 변경
    - 특수 리다이렉션 - 결과 대신 캐시를 사용
- 영구 리다이렉션 - 301, 308
    - 원래의 URL을 사용X, 검색 엔진 등에서도 변경 인지
    - 301 Moved Permanently
        - 리다이렉트 시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
        - e.g., old-page에서 별도의 폼을 입력했다고 해도 GET으로 변하고, new-page에서도 body가 없어져서 다시 입력해야 함
    - 308 Permanent Redirect
        - 301과 기능은 같음
        - 리다이렉트 시 요청 메서드와 본문 유지, 다만 대체로 페이지가 변경되면 보내야 되는 본문도 변하는 경우가 많아서 사용 빈도가 적음
- 일시적인 리다이렉션 - 302, 307, 303
    - 302 Found
        - 리다이렉트 시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
    - 307 Temporary Redirect
        - 302와 기능은 같음
        - 리다이렉트 시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다. MUST NOT)
    - 303 See Other
        - 302와 기능은 같음
        - 리다이렉트 시 요청 메서드가 GET으로 변경
- PRG: Post/Redirect/Get - 일시적인 리다이렉션
    - POST로 주문 후에 새로고침으로 인한 중복 주문 방지
    - POST로 주문 후에 주문 결과 화면을 GET 메서드로 리다이렉트
    - 새로고침해도 결과 화면을 GET으로 조회
    - 중복 주문 대신에 결과 화면만 GET으로 다시 요청
- 기타 리다이렉션
    - 300 Multiple Choices: 안 씀
    - 304 Not Modified - 캐시를 목적으로 사용, 클라이언트에게 리소스가 수정되지 않았음을 알려줌, 304 응답은 메시지 바디를 포함하면 안됨(로컬 캐시를 사용해야 하므로)

### 4xx - 클라이언트 오류, 5xx - 서버 오류

- 4xx (Client Error) - 오류의 원인이 클라이언트에 있음, 이미 잘못된 요청으로 재시도가 실패함
    - 400 Bad Request - 잘못된 요청으로 서버가 처리할 수 없음, 요청 구문, 메시지 등등 오류, 요청 파라미터가 잘못되거나 API 스펙과 맞지 않을 때
    - 401 Unauthorized - 클라이언트가 해당 리소스에 대한 인증이 필요, 인증(Authentication)과 인가(Authorization) 모두 포함
    - 403 Forbidden - 서버가 요청을 이해했지만 승인 거부, 인증 자격 증명은 있지만 접근 권한이 없음
    - 404 Not Found - 요청 리소스가 서버에 없음, 또는 클라이언트가 권한이 부족한 리소스에 접근했을 때 아예 숨기고 싶으면 404를 내리기도 함
- 5xx (Server Error) - 서버 문제로 오류 발생, 서버에 문제가 있기 때문에 재시도하면 성공할 수 있음
    - 500 Internal Server Error - 서버 내부 문제로 오류 발생, 애매하면 500 오류
    - 503 Service Unavailable - 서버가 일시적인 과부하 또는 예정 작업으로 잠시 요청을 처리할 수 없음

### HTTP 헤더 개요

- HTTP 전송에 필요한 모든 부가정보
- RFC2616(과거)
- HTTP 표준
    - 1999년부터 사용된 RFC2616 폐기
    - 2014년 RFC7230~7235 등장
- RFC723x 변화
    - 엔티티(Entity) → 표현(Representation)
    - Representation = representation Metadata + Representation Data
    - 표현 = 표현 메타데이터 + 표현 데이터

### 표현

- 표현이라는 건 db에서 데이터를 꺼내서 클라이언트와 서버가 주고 받을 때 HTML이든 JSON이든 그 외 어떤 ‘표현’이 될 수 있다는 느낌으로 이해하면 좋음
- 표현 헤더는 전송, 응답 둘 다 사용
    - Content-Type: 표현 데이터의 형식
    - Content-Encoding: 표현 데이터의 압축 방식
    - Content-Language: 표현 데이터의 자연 언어
    - Content-Length: 표현 데이터의 길이
- Content-Type
    - 미디어 타입, 문자 인코딩
    - e.g., text/html; charset=utf-8 / application/json / image/png
- Content-Encoding
    - 표현 데이터를 압축하기 위해 사용, 전달하는 곳에서 압축 후 인코딩 헤더 추가, 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제 e.g., gzip, deflate, identity
- Content-Language
    - 표현 데이터의 자연 언어를 표현 e.g., ko, en, en-US
- Content-Length
    - 바이트 단위
    - Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안 됨

### 콘텐츠 협상(Content Negotiation)

- 클라이언트가 선호하는 표현 요청, 협상 헤더는 요청 시에서만 사용
- 콘텐츠 협상 종류
    - Accept: 클라이언트가 선호하는 미디어 타입 전달
    - Accept-Charset: 클라이언트가 선호하는 문자 인코딩
    - Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
    - Accept-Language: 클라이언트가 선호하는 자연 언어
- 협상과 우선순위1 - Quality values(q)
    - Quality values(q) 값 사용, 0~1 클수록 높은 우선순위이고 생략하면 1
    - e.g., Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
- 협상과 우선순위2 - Quality values(q)
    - 구체적인 것이 우선 순위가 높다.
    - Accept: text/*, text/plain, text/plain;format=flowed, */*
        1. text/plain;format=flowed
        2. text/plain
        3. text/*
        4. */*
- 협상과 우선순위3 - Quality values(q)
    - 구체적인 것을 기준으로 미디어 타입을 맞춘다.

### 전송 방식

- 단순 전송 - Content-Length에 맞게 한번에 요청하고 한번에 받음
    - Content-Length: 3423
- 압축 전송 - gzip같은 걸로 압축해서 전송, 명시된 Content-Encoding을 확인하게 압축 해제
    - Content-Encoding: gzip
- 분할 전송 - 5바이트씩 쪼개서 보내거나 등, Content-Length는 보내면 안됨
    - Transfer-Encoding: chunked
- 범위 전송
    - Content-Range: bytes 1001-2000 / 2000

### 일반 정보

- From - 유저 에이전트의 이메일 정보, 일반적으로 잘 사용되지 않음
- Referer - 현재 요청된 페이지의 이전 웹 페이지 주소, 유입 경로 분석 가능, referrer의 오타
- User-Agent - 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등)
- Server - 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
    - 캐시 서버나 프록시가 아닌 진짜 요청을 받는 서버의 정보
- Date - 메시지가 발생한 날짜와 시간, 응답에서 사용

### 특별한 정보

- Host - 요청한 호스트 정보(도메인), 필수값
- Location - 페이지 리다이렉션
    - 웹 브라우저는 3xx 응답 결과에 Location 헤더가 있으면 Location 위치로 자동 이동(리다이렉트)
    - 201: Location 값은 요청에 의해 생성된 리소스 URI
- Allow - 허용 가능한 HTTP 메서드
    - e.g., GET, PUT만 가능한 API에 POST를 날리면 서버에서 405(Method Not Allowed) 에서 Allow: GET, PUT 과 같이 응답을 할 수 있음, 거의 사용되지 않음
- Retry-After - 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
    - 503(Service Unavailable)일 때 서비스 재개 가능 시점을 알려줄 수 있음
    - Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
    - Retry-After: 120 (초단위 표기)
    - 거의 사용되지 않음

### 인증

- Authorization - 클라이언트 인증 정보를 서버에 전달
- WWW-Authenticate - 리소스 접근 시 필요한 인증 방법 정의, WWW-Authenticate 값을 보고 제대로 된 인증 정보를 만들라는 의미
    - 401 Unauthorized 응답과 함께 사용
    - WWW-Authenticate: Newauth realm=”apps”, type=1, title=”Login to \”apps\””, Basic realm=”simple”

### 쿠키

- 모든 요청에 쿠키 정보 자동 포함
- 서버에서 쿠키 세팅하는 예시) set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure
- 사용처 - 사용자 로그인 세션 관리, 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨 - 네트워크 트래픽 추가 유발, 최소한의 정보만 사용(세션 id, 인증 토큰), 서버에 전송하지 않고 클라이언트 내에서만 사용하고 싶으면 웹 스토리지 사용 가능
- 민감 정보는 저장해선 안됨(주민번호 등 개인 정보)
- 쿠키 - 생명주기 Expires, max-age
    - Set-Cookie: expires=Sat, 26-Dec-2020 04:38:21 GMT - 만료일이 되면 쿠키 삭제
    - Set-Cookie: max-age=3600 (3600초) - 0이나 음수로 지정하면 쿠키 삭제
    - 세션 쿠키 - 만료 날짜를 생략하면 브라우저 종료 시 삭제
    - 영속 쿠키 - 만료 날짜를 입력하면 해당 날짜까지 유지
- 쿠키 - 도메인
    - 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
        - dev.example.org 접근 가능
    - 생략: 현재 문서 기준 도메인만 적용
        - dev.example.org 접근 불가
- 쿠키 - 경로
    - 예) path=/home
    - 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
    - 일반적으로는 path=/ 루트로 지정
- 쿠키 - 보안
    - Secure - 쿠키는 http, https 구분하지 않지만 Secure 적용하면 https인 경우에만 전송
    - HttpOnly - XSS 공격 방지, 자바스크립에서 접근 불가(document.cookie), HTTP 전송에만 사용
    - SameSite - XSRF 공격 방지, 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송

### 캐시 기본 동작

- 서버에서 캐시 설정: cache-control: max-age=60 → 응답 결과를 캐시에 저장, 60초 유효
- 재차 요청할 때 캐시를 조회하고 있으면 캐시에서 가져옴
- 캐시 시간 초과 → 네트워크 다운로드 후 응답 결과를 다시 캐시에 저장

### 검증 헤더와 조건부 요청 1

- 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타남
    - 데이터가 다른 경우 - 서버에서 기존 데이터를 변경
    - 데이터가 여전히 같은 경우 - 서버에서 기존 데이터를 변경하지 않음
- 캐시 시간 초과
    - 캐시 시간이 초과되었더라도 캐시 데이터를 사용 가능, 다만 동일한 데이터인지 검증 필요
- 검증 헤더 추가
    - Last-Modified: 2020년 11월 10일 10:00:00
    - Last-Modified 헤더를 통해 받은 데이터를 캐시할 때 캐시에 같이 기록
    - 재차 http 요청을 보낼 때 if-modified-since 요청 헤더를 통해 조건부 요청 보냄
    - 해당 값을 통해 서버에서 검증 가능
    - 값이 동일하면 HTTP/1.1 304 Not Modified + 큰 용량의 body 정보 없이 header 정보만 response

### 검증 헤더와 조건부 요청 2

- If-Modified-Since를 통해 조건부 요청
- Last-Modified, If-Modified Since 조건부 요청의 단점
    - 1초 미만(0.x초) 단위로 캐시 조정이 불가능
    - 날짜 기반의 로직이다 보니 실제 컨텐츠가 변경되지 않아도(A→B, B→A로 수정했다면) 캐시 유지 불가능
    - 서버에서 캐시 메커니즘을 컨트롤할 수 있는 방법이 ETag
- ETag, If-None-Match
    - Entity Tag
    - 서버에서 받은 ETag를 캐시에 If-None-Match에 저장함
    - 진짜 단순하게 ETag가 같으면 유지, 다르면 다시 받기
    - 클라이언트는 캐시 매커니즘을 알 수 없음, 클라이언트는 단순히 이 값을 서버에 제공

### 캐시와 조건부 요청 헤더

- Cache-Control
    - Cache-Control: max-age - 캐시 유효 시간, 초 단위
    - Cache-Control: no-cache - 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
    - Cache-Control: no-store - 데이터에 민감한 정보가 있으므로 저장하면 안됨(메모리에서 사용하고 최대한 빨리 삭제)
- Pragma - 하위 호환
- Expires - 캐시 만료일 지정, 현재는 더 유연한 Cache-Control: max-age 권장

### 프록시 캐시

- Cache-Control
    - Cache-Control: public - 응답이 public 캐시에 저장 가능
    - Cache-Control: private - 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)
    - Cache-Control: s-maxage - 프록시 캐시에만 적용되는 max-age
    - Age: 60 (HTTP 헤더) - 오리진 서버에서 응답 후 프록시 캐시 내에 머무는 시간 설정

### 캐시 무효화

- Cache-Control: no-cache, no-store, must-revalidate
- Pragma: no-cache - HTTP 1.0 하위 호환
- Cache-Control: must-revalidate - 캐시 만료 후 최초 조회 시 원 서버에 검증해야 함
- no-cache vs must-revalidate
    - no-cache - 원 서버에 접근할 수 없는 경우 서버 설정에 따라서 캐시 데이터를 반환할 수 있음, 오류보다 오래된 데이터라도 보여주자
    - must-revalidate - 원 서버에 접근할 수 없는 경우 항상 오류 발생, 매우 중요한 돈과 관련된 결과 등

### 다음으로

- HTTP 스펙
    - RFC 2616 - 이건 구스펙을 하단 참고
    - RFC 7230~7235 - 이걸로 모두 개정
- HTTP 완벽가이드 도서