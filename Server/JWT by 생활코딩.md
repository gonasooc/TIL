- JWT = Json Web Token
- payload = 내용, payload의 각 부분의 claim
- signature = 내용에 대한 서명
- 각각의 header, payload, signature를 base64로 인코딩해서 연결된 것이 JWT
- 인증
  - 쿠키 기반의 인증
    - 로그인 시도 → App Server에서 database로 접근해서 처리가 필요
  - jwt 기반의 인증
    - 서버의 부담을 줄일 수 있음
