### 드라마앤컴퍼니 채용의 혁신: Amazon OpenSearch Service로 열어가는 미래

- 생성형 ai와 검색 기술의 진화
    - 검색에도 큰 영향을 끼침 - 비정형 데이터를 검색할 수 있게 됨
    - 생성형 Ai 모델로 데이터를 벡터화하면 데이터 유형에 상관없이 다양한 검색 기법이 적용 가능
- 벡터와 벡터 데이터베이스
    - 벡터는 데이터를 기계 학습 모델이 숫자의 배열로 변환한 것 embedding
    - ex) 아마존 → 벡터로 표현하면 다른 숫자로 표기 가능
    - 임베딩된 벡터 사이의 거리(knn)을 통해 데이터 사이의 유사성 파악
    - 핵심은 대용량의 벡터 데이터를 빠르게 검색할 수 있는 엔진
- 현대적인 검색 예시 - 시맨틱 멀티모달 대화형 검색
    - 사진 속 남자가 신고 있는 신발은 무엇인가요 → 그 신발 어디서 살 수 있나요 (히스토리 저장)
- 개인화 추천 - Amazon Music

---

- 리멤버 채용 서비스의 현황과 과제
    - 86%의 경력직은 채용 중인 포지션 존재를 미인지
    - 때문에 채용담당자가 적합한 인재를 찾을 수 있는 검색의 혁신이 필요했음
- 새로운 채용 서비스의 목표
    - 필터
    - 필터를 걸지 않더라도 직무기술서를 바탕으로 맞춤 인재 추천
    - 인재 타겟 채용 공고 알림 → 직무기술서에 맞는 인재에게만 공고를 노출하고 맞춤 인재에게 메시지 전송
- 왜 벡터 검색 엔진이 필요한가?
    - Keyword 가 아닌 이유 → 개발자를 검색했을 때 모든 개발자가 검색되는 것이 아니라 맥락을 파악한 시맨틱 검색이 필요함, 직무/경력의흐름/자기소개 등의 데이터를 고려한 맥락을 파악
- 벡터 엔진 - AI 채용 비서의 핵심 기술
    - 400만개의 프로필 → 임베딩 모델로 벡터화(프로필을 하나의 좌표로 설정) → 벡터 검색 엔진을 활용(오픈서치)
- 벡터 엔진의 필수 조건
    - 400만개 이상 저장 가능
    - 실시간으로 knn 서치를 처리할 수 있는 속도
    - 추가 정보를 활용한 필터링 기능
    - 큰 차원의 임베딩 벡터 연산 가능
- 도입시 고려했던 벡터 엔진
    - Document DB, ElasticSearch, Other Vector DB, OpenSource
- 앙상블 모델
- 리멤버의 임베딩 모델 학습 과정
    - [채용공고 + 적합 프로필 + 부적합 프로필] 형태의 데이터셋 생성
    - 적합한 것은 더 가깝게, 부적합한 건 더 멀게 되도록 지도학습(Supervised Contrastive Learning)
- RAG과 대화형 검색 도입 계획
    - 검색 쿼리와 유사한 결과 제공 목적
- 포지션 추천 자동 메일 전송 서비스 도입 계획
    - Amazon Bedrock Claude가 사전 정의된 내용

### CJ 제일제당의 AI 서비스 기반 제조 혁신

- 소비재(CPG) 제품은?
    - 높은 생산 속도와 처리량
    - 다양한 카테고리 몇 변동성
    - 더 까다로운 품질 기준
- 비전 검사의 기존 접근법
    - 인적 검사, 육안 검사 - 처리 속도 및 일관성 부족
    - 규칙 기반 비전 검사 - 생산 환경이나 제품 스펙이 달라졌을 때 유연성 부족
    - 머신러닝 기반 비전 검사 - 높은 기술 난이도
- 머신러닝 비전 검사의 한계
    - 전문가와 외부 업체에 대한 의존도가 높음, 유지보수에 대한 상당한 제약
- 해결방안: 현장 주도적 업무 체계 구축
    - NO 코드 머신러닝으로 현장 주도적 ML 파이프라인 구축
    - 현장 전문가가 사용 가능한 난이도와 오프라인 환경에 배포 및 활용 가능(ex) 식품공장의 경우 물을 많이 사용함), 손쉬운 장치 관리 및 검사 데이터 중앙화
- AWS 솔루션 아키텍처 도출
    - Amazon Lookout for Vision (L4V)
- 노코드 기반 머신러닝 프로세스
- 최종 아키텍처
    - SageMaker, Lookout for Vision
- 현장 주도적 머신러닝 프로세스 장점
    - 개발 기간 3개월 단축, 혁신적인 학습 및 배포, ML 내재화 지속적 모델 개선

### 생성형 AI 시대의 고객 데이터 플랫폼 구축 전략

- They knew what you need next
    - 부모도 모르는 딸의 임신 대형 마트는 알고 있었다
    - 구매 버튼을 누르기 전 아마존은 다음 구매 상품은 준비중
- 리테일 산업의 목표
    - consumer, taste, marketing
- 트렌드 키워드: 초개인화
    - 양면적 소비자(Ambisumer) - 합리적인 소비를 하지만, 취향(경험) 소비는 과감하게
    - 디토’ 소비(Ditto)
- 초개인화 마케팅 사례 - 고객 정보 기반으로 화장품과 스타일 추천
- 초개인화 마케팅을 위한 GenAI 기술의 활용
    - AI 대화형 챗봇, AI 카피라이터, AI 큐레이터

### AWS로 구현하는 혁신적 애플리케이션을 위한 인프라 구축

- AWS Nitro System
- AWS Graviton4

### 다양한 측면에서 보는 Observability

- ex) end-customer를 위한 이커머스 서비스
- 메트릭의 구성
- 4가지의 골든 시그널 - LETS
    - Latency
    - Errors
    - Traffic
    - Saturation
- 로그 - 이벤트에 대한 기록

### Amazon OpenSearch 서비스의 벡터 기반 RAG 구축을 통한 생성형 AI 검색 성능 향상

- RAG(Retrieval Augmented Generation)란?
    - 파운데이션 모델이 갖고 있지 않은 외부 벡터 임베딩을 통해 검색 능력 향상
- JSON으로 OpenSearch에 전송 → 문서를 인덱스로 전송 → 인덱스는 샤드 구조로 동작 → 각각의 필드는 토큰화
- 쿼리 실행: 쿼리를 소문자 변환, 특문 제거, 공백 제거
- Okapi BM25를 사용한 랭킹
    - 확률 검색 모델 기반
    - 용어 출현 빈도, 문서 길이, 단어의 회소성 고려
    - 짧은 문서에서 일치 시 높은 점수, 긴 문서에서 일치 시 페널티 부여
- 벡터 엔진 핵심 개념
    - 백터: 값들의 배열
    - 모델ㅣ 입력 → 출력 관계를 학습하는 함수
    - LLM: 방대한 언어 데이터를 학습하여 자연어를 이해하고 생성할 수 있는 AI
    - 임베딩 - 텍스트를 벡터로 변환
- KNN의 경우 데이터가 많아지면 복잡도가 증가 → ANN 알고리즘

---

- RAG
    - 다양한 pdf를 Titan Embedding을 통해 벡터 임베딩 후 Amazon OpenSearch에 저장
        - Search API를 통해서 확인 가능
    - Amazon Bedrock을 통해 사용자의 용어 질문 및 요청을 받아서 SQL 생성
    - SQL 질의 → Amazon Athena에서 Federated 질의를 통해 여러 공간에 검색 및 질의

### Imply Druid를 활용한 실시간 데이터 분석과 당근마켓 사례

- 이벤트 데이터 기반 메시지의 표준화 - 스트리밍 데이터 대응
- 스트리밍 데이터의 배치 전환은 생산성을 저하 → Druid는 실시간 데이터 분석을 위해 설계됨

---

- 전문가모드 플랫폼 - 다양한 옵션을 제공한다면 그에 걸맞는 성과 지표가 필요함
    - 다차원 실시간 성과 조회
    - 성별/연령/관심사/OS 타겟팅
    - 커스텀 타겟팅
    - MAT 연동, 광고주 타겟 파일, 픽셀, etc…