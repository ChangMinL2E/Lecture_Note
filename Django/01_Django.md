# Django 실습

---  
- 가상환경(venv)    
: 가상환경을 만들어주는 파이썬 라이브러리(venv)  
  
&rightarrow; 가상환경 만들기  
```python
# 가상환경 만들기
python -m venv venv # 대부분 가상환경 이름으로 venv로 일치한다.
```
  
&rightarrow; 가상환경 실행시키기  
```python
# 실행  
source venv/Scripts/activate  
```
  
- django 및 필요 패키지 목록 생성 및 설치  

```python
# 설치  
pip install -r requirements.txt  

# requirements 작성  
pip freeze > requirements.txt  
```  

- Project 생성  
```python
django-admin startproject firstpjt .
```  

- 서버 실행  
```python
python manage.py runserver  
```
  
- 앱 생성
: 일반적으로 복수형 이름으로 작성함
```python
python manage.py startapp articles 
```

- 어플리케이션 등록  
```python
# pjt/settings.py

INSTALLED_APPS = [
    'articles',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```  
&Rightarrow; 먼저, 작성하고 생성하면 앱이 생성되지 않는다.  

---  
### 요청과 응답  

- URL &rightarrow; VIEW &rightarrow; TEMPLATE   

```python
#pjt/urls.py  
from django.contrib import admin
from django.urls import path
from articles import views # articles에 views.py

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/',views.index)
]
```  
```python
# articles/views.py  

def index(request): # 인자로 HTTP 요청 수신, 응답을 반환하는 함수
    return render(request, 'index.html') # Template에게 HTTP 응답 서식을 맡긴다.
```  
##### render()  
`render(request, template_name, context)`    
: 주어진 템플릿을 주어진 context 데이터와 결합하고, 렌더링된 text와 함께 응답객체(HttpResponse)를 반환하는 함수  

1. request  
: 응답을 생성하는 데 사용되는 요청 객체  
   
2. template_name  
: 템플릿의 전체 이름 or 이름의 경로  
   
3. context  
: template에서 사용할 데이터(dictionary)  
   
#### Templates  
: 실제 내용을 보여주는 파일(html)  
- Template 파일읠 기본 경로  
: app 폴더 안의 templates 폴더 `app_name/templates/`
  
```python
# articles/templates/index.html  
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>만나서 반가워요!</h1>
</body>
</html>
```  

- 추가 설정  
```python
# settings.py  

LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
```
---  
### Django Template Language  

Django template에서 사용하는 built-in template system  

#### DTL Syntax  
> 1. Variable  
> 2. Filters  
> 3. Tags  
> 4. Comments  

- Variable  
```html
{{variable}}
```
: render()의 context로 들어온 데이터 사용  

- Filters  
```html
{{variable|filter}}
```  
: 변수 수정할 때 사용  
ex) `{{name|lower}}` : name 변수를 소문자로 출력  

- Tags  
```html
{% tags %}
```
```html
{% if %}{% endif %}
```

- Comments  
```html
{# #}  

{% comment %}
{% endcomment %}
```  

#### context 데이터가 많아지는 경우 다음과 같이 작성한다.  
```python
# articles/views.py

def index(request):
    return render(request, 'index.html')  

def greeting(request):
    foods = ['apple','banana','coconut',]
    info = {
        'name':'Alice'
    }
    context = {
        'foods':foods,
        'info':info
    }
    return render(request, 'greeting.html', context)
```  

```python
# articles/views.py  
from django.shortcuts import render
import random

def dinner(request):
    foods = ['족발','햄버거','치킨','초밥',]
    pick = random.choice(foods)
    context = {
        'pick' : pick,
        'foods' : foods,
    }
    return render(request, 'dinner.html', context)
```  
- Filters example
```html
<!--articles/templates/dinner.html -->  
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <p>{{pick}}은 {{pick|length}}글자 </p>  
  <p>{{foods|join:", "}}</p>

  <p>메뉴판</p>  
  <ul>
    {% for food in foods %}
      <li>{{food}}</li>
    {% endfor %}
  </ul>

  <a href="/index/">뒤로</a>
</body>
</html>
```
---  
### Template inheritance(템플릿 상속)  

`{%extend ''%}`태그를 이용해, 부모 템플릿 상속&rightarrow; 제일 위에 작성해야 한다.  

`{%block content%}{%endblock content%}`:  overridden 할 수 있는 블록  

#### 추가 템플릿 경로 추가  
```python
# settings.py  
TEMPLATES = [
    {
        'DIRS': [BASE_DIR/'templates'],
        ...
]
```   
---  
### Sending form data (Client)  
- HTML `<form>` element  
: "데이터를 어디(action)로 어떤 방식(method)로 보낼지"  
    >- action  
    >    - 전송될 url
    > 
    >- method  
    >    - 어떻게 보낼 것인지 HTTP request methods를 지정  
    >    - GET, POST 방식  
    
- `<input>` element  
: type 속성에 따라 동작 방식이 달라진다.(기본값 : "text")  
  
> - name  
> : form을 통해 데이터 submit 하면, name 속성에 설정된 값을 서버로 전송한다.  
> name은 key, value는 value로 매핑한다.  
> GET에서 `?key=value&key=value/`형식으로 데이터 전달  

```html
<!-- articles/templates/throw.html-->
{% extends 'base.html' %}

{% block content %}
  <h1>Throw</h1>
  <form action="#" method = "#">
    <label for="message">Throw</label>
    <input type="text" id = "message" name = "message">
    <input type="submit">
  </form>
{% endblock content %}
```  

#### HTTP request methods  
- HTTP  
: 웹에서 이루어지는 모든 데이터 교환의 기초  
  주어진 리소스가 수행 할 원하는 작업을 나타내는 request methods 정의  
  
- HTTP request methods  
: 자원에 대한 행위 정의  
  자원에 수행하길 원하는 행동  
  - GET, POST, PUT, DELETE  
    
#### GET  
- 서버로부터 정보를 조회하는 데 사용  
    - 서버에게 리소스 요청하기 위해 사용  
    
- 데이터 가져올 때만 사용  

---  
### Retrieving the data (Server)  

```python
# pjt/urls.py  
urlpatterns = [
    ...
    path('catch/',views.catch),
]
```
```python
# articles/views.py
def catch(request):
    return render(request, 'catch.html')
```  
```html
<!--articles/templates/throw.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>Throw</h1>
  <form action="/catch/" method = "GET">
    <label for="message">Throw</label>
    <input type="text" id = "message" name = "message">
    <input type="submit">
  </form>
{% endblock content %}
```  
action 작성했다.  
```html
<!--articles/templates/catch.html -->
{% extends 'base.html' %}

{% block content %}
  <h1>Catch</h1>
  <h2>받는곳</h2>
  <a href="/throw/">다시 돌아가자</a>
{% endblock content %}
```

- GET method로 보내면, Query String Parameters를 통해 전송  
- request.GET 은 QueryDict으로 들어오고, request.GET.get('message')가 내용이다.  

```python
# catch 함수 

def catch(request):
    message = request.GET.get('message')
    context = {
        'message' : message, 
    }
    return render(request, 'catch.html',context)

```
---  
#### Variable routing  
변수는 `<>`에 정의, view 함수의 인자로 할당(urls.py)에서 path 작성    
- str, int로 타입 명시 가능  

---  
#### App URL mapping  
: project에 app이 많아질수록 효율적인 관리 필요  
&rightarrow; 각 app 폴더 안에 urls.py 작성  

```python
# pjt/urls.py  
from django.contrib import admin
from django.urls import path, include
from articles import views


urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]
```
```python
# articles/urls.py  
from django.urls import path
# 명시적 상대경로
from . import views


app_name = 'articles'
urlpatterns = [
    path('index/', views.index, name='index'),
    path('greeting/', views.greeting, name='greeting'),
    path('dinner/', views.dinner, name='dinner'),
    path('throw/', views.throw, name='throw'),
    path('catch/', views.catch, name='catch'),
    path('hello/<str:name>/', views.hello, name='hello'),
]
```

- include()  
: 다른 URLconf들을 참조할 수 있도록 돕는 함수  
  
--- 
#### Naming URL patterns  
: path() 함수 name 인자를 정의   
&rightarrow; url의 이름 수정시, 모든 html 파일에서 수정할 필요가 없다.
&rightarrow; url tag를 사용해서, path 함수에 작성한 name을 사용  

- url tag  
```python
# articles/templates/articles/catch.html
{% extends 'base.html' %}

{% block content %}
  <h1>Catch</h1>
  <h2>여기서 {{ message }}를 받았다!!</h2>
  <a href="{% url 'articles:throw' %}">다시 던지러!</a>
{% endblock content %}

```
&Rightarrow; 위에서 a태그처럼 주소를 작성해야할때, url tag 작성  





