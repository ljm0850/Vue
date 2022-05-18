# Vue router & Vuex 추가

## Vue router - 404 page

### 순서

- 404 page 작성, url 설정

- router/index.js 설정

  - ```javascript
    const routes = [
        ...
        {
            path: '/404',
            name: 'NotFound404',
       		component: NotFound404
        },
        {
            path: '*',
            redirect: '/404'
        }
    ]
    ```



### 404 page가 나와야 하는 경우

- Vue Router에 등록되지 않은 routes일 경우

- Vue에 Router에 등록은 되어 있지만, 서버에 리소스가 없는 경우

  - /articles/:num 에서 num에 해당하는 정보가  서버에 없을 경우

  - ```javascript
    // Vuex
    ...
    axios.get(URL)
    	.then(res=> {...})
        .catch(err => {
            console.error(err.response)
            if (err.response.status === 404) {
                router.push({ name: 'NotFound404'})
            }
        })
    ```



## Navigation Guard

### Global Before Guards

```javascript
// router/index.js
const routes = [
    ...],
    
const router = new VueRouter({
    mode: 'history',
    base: process.env.BASE_RUL,
    routes
})

router.beforeEach((to, from, next)={
    const a = ...
    const b = ...
    if (a & b) {
    next({ name: 'login' })
} else {
    next()
}
})
export default router
```

- URL 이동할 떄 마다, 이동 전에 발생
  - 필수적으로 로그인이 필요한 경우 ...
- router 객체의 메서드로 콜백 함수를 인자로 받고, 해당 콜백함수는 to,from,next 3개의 인자를 받음
  - to : 이동하려는 route의 정보를 담은 객체
  - from: 직전 route 정보를 담은 객체
  - next: 실제 route의 이동을 조작하는 함수



## Vuex Module

### Module 분리

``` javascript
// @/store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

import accounts from './modules/accounts'

Vue.use(Vuex)

export default new Vuex.Store({
  modules: { accounts },
   // modules: {
   //    namespaced: true,
   // }
})
```

- 단일파일 store/index.js 에 모든 state, getters, mutations, actions를 작성하면 복잡
- 필요에 따라 모듈로 분리 하여 사용
- 기본적으로 모든 모듈은 index.js에서 펼쳐지는 형태 (django에서 template를 한곳에 모으는 것 처럼)
  - django에서는 각 app에 templates/articels 와 같이 폴더를 추가해서 구분
  - Vuex에선 Namespacing 적용을 통해 가능