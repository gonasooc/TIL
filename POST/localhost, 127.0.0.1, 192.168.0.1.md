### 처음 접했던 localhost

프론트 개발 환경에서 Vite와 같은 빌드 도구를 쓰다 보면 터미널에 다음과 같은 메세지가 뜬다.

```
> sayno-fe@2.0.0 dev
> vite

  VITE v5.3.4  ready in 258 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: http://192.168.0.1:5173/
  ➜  press h + enter to show help
```

간단히 말해 내 로컬 환경에서 애플리케이션이 돌아가고 있다는 뜻이다. [`http://localhost:5173/`](http://localhost:5173/로는)로는 내 컴퓨터에서만 접근할 수 있고 동일한 네트워크에 있는, 즉 같은 와이파이에 있는 다른 기기에서는 `http://192.168.0.1:5173/`를 통해 접근할 수 있다. 처음 이 안내를 접한 건 Vue 2 CLI를 통해서였고, CRA를 통해 구성하더라도 포트의 차이가 있을 뿐 기본적인 메시지의 골자는 같았다.

웹 퍼블리셔로 커리어를 시작했을 때는 저게 어떤 의미인지 이해하지 못했다. ‘왜 내 자리에서는 되는데 다른 자리에서는 안 보이지?’, ‘`192.168.0.1`로 하면 되는데 LTE로는 왜 안 될까?’ 같은 의문만 가득했다. 그냥 마감일에 쫓기면서 “일단 되니까 됐다”라는 식으로 넘어갔던 기억이 난다.

### 도메인을 IP로 변환하는 DNS

`localhost`를 이해하려다 보면 자연스럽게 DNS(Domain Name System)를 접하게 된다. DNS는 사용자가 입력한 도메인 이름을 컴퓨터가 이해할 수 있는 IP 주소로 변환하는 시스템이다. 예를 들어, 웹 브라우저에 `www.google.com`을 입력하면, DNS가 이를 해당 IP 주소로 변환하여 접속할 수 있도록 한다. 도메인은 사람이 이해하기 쉬운 표현이고, IP 주소는 컴퓨터가 이해하는, 혹은 통신하는 약속인 셈이다.

### 구글의 IP 주소 확인하기

문득 구글의 IP 주소가 궁금해졌다. 구글의 IP 주소는 터미널에서 `nslookup`, `dig`, `host` 같은 명령어로 확인할 수 있었다.

- Windows (CMD 또는 PowerShell) → `nslookup www.google.com`
- Mac/Linux → `dig www.google.com` or `host www.google.com`

`dig`는 DNS 정보를 조회하고, `host`는 `dig`보다 간단하게 조회할 때 사용한다. MacOS에서 실행한 결과는 다음과 같았다.

- `dig www.google.com`
    
    ```
    ; <<>> DiG 9.10.6 <<>> www.google.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65485
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 512
    ;; QUESTION SECTION:
    ;www.google.com.			IN	A
    
    ;; ANSWER SECTION:
    www.google.com.		15	IN	A	142.250.197.196
    
    ;; Query time: 6 msec
    ;; SERVER: 203.248.252.2#53(203.248.252.2)
    ;; WHEN: Thu Feb 13 17:53:03 KST 2025
    ;; MSG SIZE  rcvd: 59
    
    ```
    
- `host www.google.com`
    
    ```
    www.google.com has address 142.250.197.228
    www.google.com has IPv6 address 2404:6800:4005:825::2004
    ```
    

재밌는 건 `142.250.197.196`나 `142.250.197.228`를 웹 브라우저 주소창에 입력하면 구글로 접속된다는 것이다. 물론 네이버와 같이 IP 주소를 통한 직접 접속을 제한하는 경우도 있다.

### `localhost`도 도메인일까?

그렇다면 `localhost`도 도메인일까? 자신의 컴퓨터를 가리키기 때문에 다소 특별한 도메인으로 볼 수 있다. 일반적으로 `localhost`는 `127.0.0.1`로 해석되고 이를 루프백(loopback) 주소라고 한다. 루프백 주소는 컴퓨터가 스스로를 가리키는 IP 주소로, 네트워크 인터페이스를 거치지 않고 내부적인 통신을 가능하게 해준다. 루프백 네트워크 인터페이스라는 표현을 쓴다. `localhost`를 사용해서 외부 네트워크 없이 로컬에서 서비스를 실행하고 테스트할 수 있는 건 이런 이유 때문이었다.

### `localhost` !== `127.0.0.1`?

`localhost`가 `127.0.0.1`로 어떻게 해석되는지 궁금했다. DNS 없이 해석되는 이유는 운영 체제에서 특정 도메인 이름을 특정 IP 주소에 매핑하는 설정 파일을 제공하기 때문이었다. macOS에서 `/etc` 로 접근해서 `cat hosts`로 출력해 보니 아래와 같았다. `hosts` 파일은 도메인 이름을 특정 IP 주소로 직접 변환하는 역할을 하며, 이를 통해 DNS를 거치지 않아도 바로 연결될 수 있다.

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
```

위 설정을 보면 `127.0.0.1`이 `localhost`로 매핑되어 있음을 알 수 있다. 따라서 사용자가 `localhost`를 입력하면 운영 체제는 `/etc/hosts` 파일을 참조하여 이를 `127.0.0.1`로 변환하고, 루프백 인터페이스를 통해 해당 요청을 내부적으로 처리한다.

### `localhost` vs `192.168.0.1`

`localhost`는 일반적으로 IP 주소 `127.0.0.1`로 매핑되며, 앞서 말한 바와 같이 루프백 주소로 해당 컴퓨터 자체를 가리킨다. 즉, `localhost`는 외부 네트워크 없이 내부적으로 처리된다.

반면, `192.168.0.1`은 일반적으로 로컬 네트워크에서 라우터나 게이트웨이의 IP 주소로 사용된다. 이런 IP 주소는 사설 IP 주소 범위에 속하고, 로컬 네트워크 내의 장치들이 서로 통신하기 위해 사용된다. 즉, `192.168.0.1`로의 요청은 실제 네트워크 인터페이스를 통해 전달되며, 동일한 네트워크에 연결된 다른 장치와의 통신에 사용된다. 초기의 의문이었던 `192.168.0.1`이 왜 다른 컴퓨터에서는 될 수 있었는지에 대한 답이 여기 있었다.

### `localhost` vs `127.0.0.1`

그러면 `localhost`와 `127.0.0.1`는 완전히 같은 걸까? `localhost`와 `127.0.0.1`은 자기 자신을 참조한다는 점에서 기능적으로 동일하지만 표현상의 차이가 존재한다. localhost는 사람이 이해하기 쉽도록 설계된 도메인 이름이고 `127.0.0.1`는 이에 대응하는 숫자 형태의 IP 주소라는 차이점이 있다고 볼 수 있겠다.

### 참고자료

- https://pstor.dev/network-story/Difference-between-localhost-and-127-0-0-1/
- https://devocean.sk.com/blog/techBoardDetail.do?ID=165818&boardType=techBlog
- [https://velog.io/@lky9303/127.0.0.1-과-localhost의-차이](https://velog.io/@lky9303/127.0.0.1-%EA%B3%BC-localhost%EC%9D%98-%EC%B0%A8%EC%9D%B4)