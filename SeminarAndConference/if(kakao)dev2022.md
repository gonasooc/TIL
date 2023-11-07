# 웹 반응성 개선하기

[웹 반응성 개선하기: Lighthouse Userflow를 사용한 웹 반응성 개선기 / if(kakao)2022](https://youtu.be/lmjfSgkpJr8)

# Sentry를 이용한 에러 추적기, React의 선언적 에러 처리

[Sentry를 이용한 에러 추적기, React의 선언적 에러 처리 / if(kakao)2022](https://youtu.be/012IPbMX_y4?si=4Dwmouwyg1CxI9EI)

- 이슈 감지, 원인 파악, 해결까지 약 4시간 소요, 단순 원인 파악에 꽤 많은 시간 소요로 사용자 경험에 좋지 않은 결과를 줌

## PART1. 에러 추적을 개선해보자!

### Frontend에서의 에러

- 데이터 영역에서의 에러 / 화면 영역에서의 에러
- 외부 요인에 의한 에러 / 런타임 에러

### 에러 추적을 개선해보자!

- **에러 데이터 쌓기**
  - Sentry
  - 실시간 로그 취합 및 분석 도구이자 모니터링 플랫폼
  - captureException과 captureMessage 두 가지 API를 이용, 에러 데이터를 전송
  - 에러 데이터 풍부하게 쌓기 - Scope
    - configureScope로는 공통 정보를, withScope로는 각 에러 상황 시 추가 정보를 전송
  - 에러 데이터 풍부하기 쌓기 - Context
    - 이벤트에 임의의 데이터를 연결할 수 있는 context를 이용해 추가 정보 전송
    - 검색은 할 수 없고 해당 이벤트가 발생한 이벤트 로그에서 확인 가능
  - 에러 데이터 풍부하기 쌓기 - Customized Tags
    - tag는 키와 값이 쌍으로 이루어진 문자열
    - 이슈 검색 및 트래킹을 신속하게 진행 가능
  - 에러 데이터 풍부하기 쌓기 - Level
    - 이벤트마다 level을 설정하여 이벤트의 중요도 식별 가능
  - 에러 데이터 풍부하기 쌓기 - Issue Grouping
    - 모든 이벤트는 fingerprint를 가지고 있음
    - fingerprint가 동일한 이벤트는 하나의 이슈로 그룹화되며 재설정 가능
- **Severity 기준설정 및 모니터링**
  - 알람 조건 설정하기
    - When - 처음 보는 이슈가 생길 경우 등
    - If - 이벤트의 Level이 조건에 맞는 경우 등
    - Then - When과 If의 조건이 충족되면 slack, jira 등 알람
- **에러 데이터 분석**
  - 무의미한 데이터 수집은 오히려 데이터의 질을 낮출 수 있음
  - 유의미한 데이터를 수집하자
    - chunk load 에러나 network 에러는 수집 제외(timeout 에러는 수집)
    - 500 에러 - 분석하고자 하는 API의 http status를 구분하여 수집(4xx 에러는 부분적 수집 제외)
    - 에러 데이터뿐만 아니라 디버깅과 분석에 필요한 추가적인 정보 수집
