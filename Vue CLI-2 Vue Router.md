# Vue CLI-2 Vue Router

### Vue Router

- Vue.js 공식 라우터
- route에 컴포넌트를 매핑 후, 어떤 주소에서 렌더링할 지 알려줌
  - 싱글 페이지다 보니 주소로 특정 화면을 보여주기 어려운걸 해결

- **router**: 위치에 대한 경로를 지정, 이 경로를 따라 데이터를 다음 장치로 전향 시키는 장치



### Vue Router 시작

- `$ vue create ljm-router-app`
  - 전에 배운 프로젝트 생성
- `$ cd ljm-router-app`
- `$ vue add router`
  - Vue Router plugin 설치
  - App.vue를 덮어 씌우므로 그 전에 작업해둔 것이 있다면 백업 필요

```vue
<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
  </div>
</template>
```

- App.vue에 router 관련 코드 추가
- router/index.js 파일 생성
- views 디렉토리 생성



#### index.js

- 라우트 관련 정보 및 설정 작성

#### App.vue의 router-link

- 사용자 네비게이션을 가능하게 하는 컴포넌트
- 목표 경로는 `to` prop 으로 지정
- a 태그에서 GET 요청을 보내는 이벤트를 제거한 형태

#### App.vue의 router-view

- 주어진 라우트에 대해 일치하는 컴포넌트를 렌더링
- 실제 component가 DOM에 부착되어 보이는 자리

#### History mode

- HTML History API를 사용하여 router 구현한 것
- 브라우저의 히스토리는 남기지만 실제 페이즈는 이동x
  - 페이지 로드 없이 URL탐색 가능



#### Named Routes

```vue
// index.js
const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
...
```

```vue
// App.vue
<router-link to="/">Home</router-link>
<router-link :to="{ name: 'home'}">HOME</router-link>
```

- 이름을 가지는 라우트

#### 프로그래밍 방식 네비게이션

```vue
<router-link to="..."></router-link>
$router.push(...)
----
메서드 내에서 함수
router.push('home')
router.push({ path: 'home' })
...
```

- router의 인스턴스 메서드를 사용하는 방식
- 버튼 클릭시 메서드 실행 -> 메서드의 함수에 의해 이동되는 형식 



#### Dynamic Route Matching

```javascript
// index.js
const routes = [
    {
        path: '/user/:userId',
        name: 'User'
        component: User
    },
]
```

- 동적 인자 전달
- 주어진 패턴을 가진 라우트를 동일한 컴포넌트에 매핑해야 하는 경우 사용
  - user id에 따라 값이 바뀔때.. 등

- 동적 인자는 `:`으로 시작
- **.vue**에서 `this.$route.params`로 object 가져오기 가능 

```javascript
<script>
    export default{
		name: 'UserProfile',
        data: function() {
			return {
                user: this.$route.params,
            }}
}
```



### components & views

#### App.vue

- 최상위 컴포넌트

#### views/

- router에 매핑되는 컴포넌트를 모아두는 폴더

#### components/

- router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더