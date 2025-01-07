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
- 이런 IP 프로토콜의 한계를 TCP/UDP를 통해 해결

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
    
    (TCP 3 handshake를 통해 가상 연결로 확인)
    
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