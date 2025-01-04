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