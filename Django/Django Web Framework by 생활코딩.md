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

- 사용자가 지정한 경로를 누가 처리할 것인가 → 라우팅
- project의 urls.py가 특정 app으로 위임하고, app 내의 urls.py가 view 안에 각각의 함수로 위임해서 작동하는 방식
- `urlpatterns` 라는 리스트를 정의해야 함
- myproject/urls.py
  ```python
  from django.contrib import admin
  from django.urls import path, include

  urlpatterns = [
      path("admin/", admin.site.urls),
      path('', include('myapp.urls'))
  ]
  ```
- myapp/urls.py
  ```python
  from django.urls import path
  from myapp import views

  urlpatterns = [
      path('', views.index),
      path('create/', views.create),
      path('read/<id>/', views.read)
  ]
  ```
- views.py
  ```python
  from django.shortcuts import render, HttpResponse

  def index(request):
      return HttpResponse('Welcome!')

  def create(request):
      return HttpResponse('Create')

  def read(request, id):
      return HttpResponse('Read'+id)
  ```

## 장고를 쓰는 이유

- Web Server like apache, nginx, IIS (**STATIC**)
  - 웹페이지를 이미 생성해둬야 함
  - 미리 준비된 페이지로 접근
  - 이미 준비된 페이지를 호출하기 때문에 성능이 빠름, but 유지보수 불편
- Web Application Server like django, flask, php, jsp, ROL (**DYNAMIC**)
  - view.py
  - 접근하는 순간 db에 접근해서 페이지를 만들어서 반환
  - 웹서버에 비해 상당히 느림, but 유지보수가 편함

## 장고 읽기 구현

```python
from django.shortcuts import render, HttpResponse

topics = [
    {'id': 1, 'title': 'routing', 'body': 'Routing is ...'},
    {'id': 2, 'title': 'view', 'body': 'View is ...'},
    {'id': 3, 'title': 'model', 'body': 'Model is ...'},
]

def index(request):
    global topics
    ol = ''
    for topic in topics:
        ol += f'<li><a href="/read/{topic["id"]}">{topic["title"]}</a></li>'
    return HttpResponse(f'''
    <html>
    <body>
        <h1>Django</h1>
        <ol>
            {ol}
        </ol>
        <h2>Welcome</h2>
        Hello, Django
    </body>
    </html>
    ''')

def create(request):
    return HttpResponse('Create')

def read(request, id):
    return HttpResponse('Read'+id)
```

## 장고 생성 기능 구현
