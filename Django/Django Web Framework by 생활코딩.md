## 장고 설치

- python3 -m pip install django → 장고 설치
- django admin → admin 명령어 확인
- [settings.py](http://settings.py) → 프로젝트 운영에 필요한 여러 가지 설정
- [urls.py](http://urls.py) → 사용자가 접속하는 path에 따라서 어떻게, 누가 처리할 것인가 결정, 라우팅
- [manage.py](http://manage.py) → 유틸리티 파일
- python3 [manage.py](http://manage.py) → 관련 명령어 확인
- python3 [manage.py](http://manage.py) runserver → 로컬에서 실행
- python3 [manage.py](http://manage.py) runserver 8888 → 임의의 포트를 적어주면 해당 포트로 실행

## 포트의 개념

- 컴퓨터 하나에 여러 개의 서버 소프트웨어가 있다고 가정했을 때 각각의 소프트웨어와 통신하기 위해 포트라는 개념이 필요
- 0~65535개의 포트가 가능
- 해당 포트를 할당하고(리스닝) 그 포트로 각각 접속

## 장고 앱 만들기

### 프로젝트의 구성

- project 안에 app이라는 작은 단위를 만들어서 구성하게 됨
- 각각의 app 안에는 urls.py가 담기게 됨
- app 안에는 view가 있고 그 안에는 각각의 def로 정의되어 있음

### 프로젝트 흐름

- 프로젝트로 사용자가 들어오면 [urls.py](http://urls.py)를 통해서 app으로 위임
- app 안에 urls.py를 통해 적당한 view의 적당한 함수로 위임
- DB는 직접 접속하는 것이 아니라 장고 안에 model이라는 수단을 통해 DB를 사용하게 됨
- 그 결과 최종적으로 html, json, xml 등을 만들어서 사용자에게 보여주게 됨

### 앱 생성

- django-admin startapp <app-name>

## 장고 라우팅 - (URL conf)

- project의 urls.py가 특정 app으로 위임하고, app 내의 urls.py가 view 안에 각각의 함수로 위임해서 작동하는 방식
