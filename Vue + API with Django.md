# Vue + API with Django

### django-cors-headers

- `$ pip install django-cors-headers`

- 응답에 CORS header를 추가해주는 라이브러리
- 다른 출처에서 보내는 Django 애플리케이션에 대한 브라우저 요청을 허용



#### 순서

- https://github.com/adamchainz/django-cors-headers

- 설치
  - `$ pip install django-cors-headers`
- settings.py
  - `INSTALLED_APPS`에 `corsheaders`추가
  - `MIDDLEWARE`에 CommonMiddleware보다 위의 위치에 `corsheaders.middleware.CorsMiddleware` 추가
  - `CORS_ALLOWED_ORIGINS = [ 'http://주소.주소']`
    - 위 문구를 추가
    - 모두 가능하게 하려면 `CORS_ALLOW_ALL_ORIGINS = True`



## Authentication & Authorization

### Authentication

- 인증, 입증
- 사용자가 누구인지 확인
- 401 Unauthorized
  - 미승인(HTPP표준), 비인증을 의미



### Authorization

- 권한 부여, 허가
- 사용자에게 특정 리소스, 기능에 대한 액세스 권한을 부여하는 절차
- 403 Forbidden



## DRF Authentication

### 인증방식

#### Session Based

#### Token Based

- Basic Token
  - 기존에 django에서 쓰던 방식
  - 정보를 넘겨주면 server에서 확인 후 토큰 발행
- JWT
  - JSON Web Token
  - JWT 자체에 인증에 필요한 정보를 담아 가지고 있음
    - DB table과 비교 안해도 됨
    - 토큰 탈취시 문제 발생(Basic Token 방식은 db에서 인증 정보를 없애면 인증이 불가)

#### Oauth

- google
- facebook
- github



### dj-rest-auth & django-allauth 라이브러리

- https://www.django-rest-framework.org/api-guide/authentication/

#### 설치

- `$ pip install django-allauth`
- `$ pip install dj-rest-auth`

#### settings.py

- `INSTALLED_APPS`
  
  - rest_framework
  - rest_framework.authtoken
  - dj_rest_auth
  - dj_rest_auth.registration
  - allauth
  - allauth.account
  - django.contrib.sites
- `SITE_ID = 1`
  - django.contrib.sites 에서 등록 필요

- ```python
  REST_FRAMEWORK = {
      'DEFAULT_AUTHENTICATION_CLASSES': [
          'rest_framework.authentication.TokenAuthentication'
      ],
      'DEFAULT_PERMISSION_CLASSES': [
          'rest_framewrok.permissions.IsAuthenticated',
      ]
  }
  ```



#### urls.py

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    ...
    path('api/v1/accounts/', include('dj_rest_auth.urls')),
    pathA('api/v1/accounts/signup/', include('dj_rest_auth.registration.urls'))
]
```

