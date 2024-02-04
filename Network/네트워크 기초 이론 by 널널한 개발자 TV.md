[네트워크를 배우려는 사람들을 위해](https://www.youtube.com/watch?v=k1gyh9BlOT8&list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy&index=2)

# \***\*네트워크를 배우려는 사람들을 위해\*\***

![스크린샷 2024-01-12 오전 7.22.42.png](../assets/5f1b50ea60c8.png)

- 크게 보자면 User mode(S/W) / Kernel mode(S/W) / H/W
- 분류 방식, 주체에 따라 OSI 7 layer, DoD 4 layer(TCP/IP 4 layer)
- **Kernel의 구성요소를 유저모드 어플리케이션으로 추상화할 때는 File의 형태로 추상화하는데, 이때 이 File은 File이라고 부르지 않고 Socket이라고 부름**
- **각 계층의 식별자가 있음(Access → MAC, Network → IP 주소, Transport → Port 번호)**

# MAC주소, IP주소, Port번호가 식별하는 것

- MAC 주소는 NIC(LAN 카드)에 대한 식별자, IP 주소는 Host에 대한 식별자
- Host란 인터넷에 연결된 컴퓨터
- 랜카드는 유선, 무선이 있을 텐데 각각의 NIC은 MAC 주소가 부여되므로 노트북에 두 개의 랜카드가 있다면 두 개의 MAC 주소를 보유함
- 하나의 컴퓨터가 있다면 IP 주소가 몇 개나 있을까 → n개 → NIC 하나에 여러 개의 IP 주소를 바인딩할 수 있음
- MAC 주소는 하드웨어 주소인데, MAC 주소는 변경 가능한가 → 변경 가능
- Port 번호는 상황이나 처한 업무에 따라서(계층에 따라서) Process, Service, Interface 등 식별에 대한 대상이 달라짐

# Host, Switch, Network 이들의 관계에 대해…

![Untitled](../assets/711de49a1f7f.png)

- **Host → 컴퓨터인데 네트워크에 연결이 되어 있으면 Host, 네트워크에 연결된 컴퓨터**
- **Host가 Network 자체를 이루는 컴퓨터라면 Switch**
  - Router (경로 선정)
  - 방화벽, IPS (보안을 위한 스위치)
- **Host가 Network 이용 주체일 때는 End point (Peer, Server, Client)**
- Network → Internet는 Router와 DNS의 집합체
- 스위치는 레이어가 높을 수록 연산이 복잡해지고 당연하게도 고가
  - ex) L3 스위치 중 가장 대표적인 것이 Router
- 인터넷 공유기란 무엇인가 → 개념적 의미로 스위치라고 볼 수 있고 분류하자면 L3 스위치 정도

# IPv4주소 체계에 대한 암기사항

- IP(Internet Protocol) 주소 → Host에 대한 식별자
- IP 주소의 버전 → IPv4, IPv6
  - IPv4: 주소 길이 32bit → 가능한 주소가 대략 43억 개
  - IPv6: 주소 길이 128bit
- IP 주소는 8bit씩 끊어서 표시!
- IP 주소는 Network ID와 Host ID로 나뉨
- 이 Network ID를 나타내는 게 서브넷마스크

# 개발자 입장에서 Port번호 이해하기

- 개발자 입장이 아니라 Port 번호는 상황이나 처한 업무에 따라서(계층에 따라서) Process, Service, Interface 등 식별에 대한 대상이 달라짐
- 16bit - 0번 포트, 65535 포트 번호는 제외하고 사용(1~65534)
- **Port 번호는 개발자 관점에서는 명백히 Process 식별자 -** 포트 번호는 Socket에 바인딩됨
  - 네트워크 전문가 관점에서는 Service 식별자
  - 네트워크 인프라 설치, 하드웨어 작업자의 관점에서는 Interface 식별자

# Switch가 하는 일은 Switching 이다.

- 인터넷은 라우터의 집합체
- 각각의 라우터는 최적화된 경로를 결정 → 라우터는 L3 스위치의 일종
- **패킷 단위의 데이터가 라우터에 도착하면 경로 선택 스위칭을 함 → 그 중 최적의 경로를 취하게 되는데, 라우팅 테이블에 근거해서 의사 결정을 함**

# 네트워크 데이터 단위 정리 (매우 중요!)

- **User모드, Application, Process, 즉 Socket 수준(File)에서는 Stream**
  - Stream 데이터는 그 끝을 알 수 없는 일렬로 쭉 늘어진 데이터
- **그걸 네트워크로 보낼 때는 자르기, 즉 Segmention이 일어나는데, 그 잘려진 조각을 Segment, 최대 크기가 정해져 있음(MSS - Maximum Segment Size)**
- **TCP 단계에서는 Segment, 그걸 인터넷 환경, IP 단계에서는 전송 가능한 형태의 Packet**
- Packet의 최대 크기는 1500 bytes (MTU - Maximum Transport Unit), 당연하게도 MSS(Maximum Segment Size)는 Packet의 최대 크기보다 작음
- **Packet을 실어 나를 때는 다시 Frame으로 인캡슐레이션(Encapsulation)**

# 네트워크 인터페이스 선택 원리와 기준

- 만약 유선/무선 두 개의 인터넷이 연결된 상태고 크롬을 통해 소켓을 연 상태라면 인터페이스는 어떤 원리로 선택이 될까? → Swiching(인터페이스 선택)
- Windows - route PRINT / MAC - netstat -r
- 인터페이스 선택의 기준은 PC 기준 메트릭 값으로 결정 → 메트릭은 쉽게 말해 비용

# 웹 서비스를 만드신 분에 대하여…

- 웹 서비스 창시자 - 티모시 버너스 리(팀 버너스 리)
- 문서(Text) + Link 의 느낌으로 문서를 확장하고, 그 확장된 문서는 HTML 문서 형식이면서 인터넷으로 연결해서 HTTP로 전달 ex) 논문의 reference
- 그렇게 연결된 모양이 거미줄 같다 하여 ‘Web’
- 문서를 다루는 모든 소프트웨어의 구조는 자료구조의 Data, 로직이 들어간 제어체계 s/w, 사람이 다루는 인터페이스(ex) GUI) 같은 구조로 되어 있음
  - 유지보수의 편의성 때문에 구분 지어서 개발

# 초창기 웹 서비스 구조

- Web(Browser) Client - Web Server가 TCP/IP로 연결되어 있고, **HTTP는 Stateless**
- 2022년 기준 HTTP 1.0, 1.1, 2.0, 3.0 까지 나와 있는데 현재 가장 많이 사용되는 건 1.1
- HTTP는 TCP/IP가 연결되어 있다는 가정해서 HTTP 통신을 진행
- TCP/IP 연결이라는 건 결국 상태를 의미하는데, HTTP는 상태가 없음 → 이게 훗날 설계적으로 문제를 일으키기도 함
- URL에서 중요한 건 Resource인데, 클라이언트와 서버는 각각의 IP 주소를 기반으로 TCP/IP를 연결한 후에 리소스를 주세요, 하고 연결하고 HTTP가 작동됨
- http request를 보내고 http response를 받음 → 여러 가지 method 중에 GET을 통해 리소스를 받게 됨
- URL을 치면 DNS을 던지게 되고 IP 주소를 알려줌, 최종적으로 문서를 받아옴
- 클라이언트가 html 문서를 가져오면,
  1. **구문 분석(Parsing) → 자료구조(비선형) DOM**
  2. **랜더링(Rendering)**
- 이렇게 문서를 가져오고 ‘원격지 문서 뷰어’ 역할을 하는 게 초기 웹의 역할

# 웹 서비스 3대 요소

- 정적인 파일들, html를 먼저 가져와서 parcing하고 필요한 css, 그 외 assets이 확인되면 순서대로 가져옴
- 초기의 웹과는 다르게 발전에 따라 상호작용을 하면서 클라이언트와 서버 사이에 문맥(상태 → 전이)이 생기고, stateless한 http의 속성과는 다르게 기억(기록) 해야 할 필요가 생김
- 그걸 서버에서는 Database를 설계하고, 처리(연산)을 담당하는 요소와 db는 SQL을 통해 연결
- 클라이언트에서도 제어 체계가 필요하고 동적인 인터렉션이 요구되자 브라우저에 연산을 위한 소프트웨어를 넣게 됨
  1. Script: Mocha → Live → JavaScript, 실행은 클라이언트 브라우저에서 함
- 결국 클라이언트 웹 구조를 이루는 3대 요소라면, 구문분석할 수 있는 Parcer(DOM 구성), 랜더링을 할 수 있는 그래픽 랜더링 엔진, 연산 주체가 되는 스크립트 엔진
- 서버에서의 기억(기록)을 위한 DB처럼 클라이언트에서는 Cookie로 기억

# LAN과 WAN을 구별하는 방법

- H/W → Phisical, S/W → Logical == Virtual
- 인터넷은 Virtual Network이라고 볼 수 있음 → 논리적인 네트워크
- 1-2계층처럼 하드웨어로 설명되는 부분을 LAN, 논리로 설명되는 네트워크가 WAN으로 구분 짓는 것도 도움이 될 수 있음
- 그런 맥락에서 LAN은 MAC주소(48bit)를 기반으로 작동, 방송주소가 적용되는 부분도 LAN으로 볼 수 있고, WAN은 IP주소를 기반으로 작동

# 패킷의 생성 원리와 캡슐화

- Socket은 파일의 일종
- Stream 데이터는 그 끝을 알 수 없는 일렬로 쭉 늘어진 데이터 - 그걸 네트워크로 보낼 때는 자르기, 즉 Segmention, 그 잘려진 조각을 Segment
- Segment이 캡슐화되면 Packet, Packet이 캡슐화되면 Frame
- Packet은 1500(MTU)인데, 다시 Header와 Payload로 나뉨
- Header는 다시 L3(IP), L4(TCP)로 나뉨 → 특별한 이유가 없다면 보통 20 bytes
- 즉, Payload는 1460 bytes
- Header는 택배로 치면 송장 같은 정보
- Payload를 들여보는 걸 DPI(ex) 도감청)
- Segment는 내용물인데 캡슐화하면 Packet, Packet을 캡슐화하면 Frame인데 택배 트럭이 일종의 Frame
- Segment는 L4, Packet은 L3, Frame는 L2 수준의 데이터 단위다, Packet의 Payload까지 뒤지면 DPI라고 함

# L2 스위치에 대해서

- L2는 MAC 주소로 스위칭(48bit)
- 네트워크는 결국 스위치의 집합체
- L2 Access는 NIC와 직접적으로 연결되는 부분, Endpoint와 직접 만나는 스위치
- L2 Access와 만나는 스위치는 L2 Distribution
- 마지막으로 모이는 건 Router, 보통 이제 Router가 전체 PC의 Gateway 역할을 해줌
- 랜선이 연결됐을 때 상향 연결이면 Uplink
  - Link-up: 녹색(연결)
  - Link-down: 연결이 끊김
