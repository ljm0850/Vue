# Vue CLI-1

## SFC (Single File Component)

### Component

- 기본 HTML element 를 확장하여 재사용 가능한 코드를 캡슐화 하는데 도움을 줌
- 유지보수를 쉽게 만들어 주며, 재사용 측면에서 좋음
- Vue에서 Component === instance === .vue 파일
  - `const app = new View({})`의 app === Component
  - 단일 .html 파일 안에서 여러 개의 컴포넌트를 만들어 개발 가능



### SFC

- Vue 컴포넌트 기반 개발의 특징
- 한개의 Component = .vue 확장자를 가진 하나의 파일 안의 코드 결과물
- 화면의 특정 영역을 나누어 HTML, CSS, JavaScript 코드를 한개의 파일(.vue)에서 관리



## Vue CLI

- Vue.js 개발을 위한 표준 도구
- 프로젝트 구성을 도와주며 확장 플러그인, GUI, Babel 등 다양한 tool 제공



### Node.js

- 자바스크립트를 브라우저가 아닌 환경에서도 구동할 수 있도록 하는 자바스크립트 런타임 환경
- Chrome V8 엔진 제공, 여러 OS에서 실행 가능



### NPM(Node Package Manager)

- 자바스크립트 언어를 위한 패키지 관리자
  - Python의 pip에 해당
- Node.js의 기본 패키지 관리자

- ` $ npm install -g @vue/cli`
  - 설치
  - **-g** : pip와 반대로 기본이 가상환경, global 설치를 위해 사용
- `$ vue --version`
  - 버전 확인용
- ` $ vue create ljm-vue`
  - 프로젝트 생성
  - 프로젝트 생성 후에 왠만하면 `$ cd ljm-vue`터미널 주소를 옮기자
- `$ npm run serve`
  - 서버 실행
- `$ vue add typescript`
  - Javascript 대신 TypeScript 사용




## Babel & Webpack

### Babel

- JavaScript compiler
- 자바스크립트 코드를 이전 버전으로 변환하는 도구
- 최신 문법을 지원하지 않는 브라우저에 적용하기 위해 존재



### Webpack

- static module bundler 중 하나
- 모듈 간의 의존성 문제를 해결하기 위한 도구
  - 모듈의 수(JS 파일의 수)가 많아져 의존성(연결성)이 깊어지면서 어디 부분이 문제인지 모르는 상황 발생
- bundler = 의존성 문제를 해결해주는 작업



### Vue 프로젝트 구조

#### node_modules

- node.js 환경의 의존성 모듈들



#### public/index.html

- Vue 앱의 뼈대 파일
- 실제로 사용자에게 제공되는 단일 html 파일



#### src

##### assets

- webpack에 의해 빌드 된 정적 파일

##### components

- 하위 컴포넌트가 위치하는 곳

##### App.vue

- 최상위 컴포넌트

##### main.js

- webpack이 빌드 시작시 가장 먼저 불러오는 entry point
- 실제 단일 파일에서 DOM과 data를 연결 했던 것과 동일한 작업이 이루어짐
- Vue 전역에서 활용 할 모듈을 등록



#### bable.config.js

- babel 관련 설정 작성된 파일

#### package.json

- 프로젝트의 종속성 목록과 지원되는 브라우저에 대한 구성 옵션

#### package-lock.json

- node_modules의 모듈과 관련된 모든 의존성을 설정 및 관리

- 배포 환경에서 정확히 동일한 종속성을 설치하도록 보장
  - 패키지 버전을 고정



## Pass Props & Emit Events

### 컴포넌트 작성

- 컴포넌트간 부모-자식 관계(트리 구성)
- 부모는 자식에게 데이터를 전달(Pass Props)
- 자식은 부모에게 일어난 일을 보고(Emit Events)
  - 보고만 가능, 보고로 인한 function은 부모에서 작성 및 실행

### 컴포넌트 구조

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- 3. 보여주기 -->
    <!-- <TheAbout my-message="CamelCase"/> -->
    <the-about my-message="kebab-case" @child-input-change="onBoss"></the-about>
  </div>
</template>

<script>
// 1. 불러오기
import TheAbout from './components/TheAbout.vue'
export default {
  name: 'App',
  components:{
    // 2. 등록하기
      TheAbout,
  },
  data: function() {
    return
  }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

- 템플릿 (HTML)
  - HTML의 body 부분
  - 각 컴포넌트를 작성
- 스크립트(JavaScript)
  - 컴포넌트 정보, 데이터, 메서드 등 Vue 인스턴스 작성
- 스타일(CSS)
  - 컴포넌트의 스타일 담당

#### 컴포넌트 등록 3단계

- 불러오기(import)
- 등록하기(register)
- 보여주기(print)



### Props

- 부모 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성
- 자식 컴포넌트는 수신하는 props를 선언해야 함
  - 상위 데이터를 직접 참조 불가



### Static Props 작성

```vue
// 부모.vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <TheAbout my-message="CamelCase"/> -->
    <the-about my-message="kebab-case" @child-input-change="onBoss"></the-about>
  </div>
</template>
```

- 자식 컴포넌트에 보낼 prop 데이터 선언
- `prop-data-name="value"`
  - 위 예시에서 `my-message="kebab-case"`에 해당
  - **`-`를 사용하여 문장 구성**

```vue
// 자식.vue
<template>
  <div>
    <h1> template에는 한개의 element만</h1>
    <h2>{{ myMessage }}</h2>
  </div>
</template>
<script>
export default {
  name: 'TestCase',
  props: {
    myMessage: String,
  },
```

- 수신 할 prop 데이터를 명시적으로 선언
  - `{{ myMessage }}` + `myMessage: String`
  - **JS와 같은 카멜케이스 형식**

##### Props 이름 컨벤션

- **선언시 camelCase**
- **in template : kebab-case**



### Dynamic Props

```vue
// 부모.vue
<template>
<div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <!-- <TheAbout my-message="CamelCase"/> -->
    <the-about my-message="kebab-case"
    :parent-data="parentData"
    @child-input-change="onBoss"></the-about>
  </div>
</template>
<script>
import TheAbout from './components/TheAbout.vue'
export default {
  name: 'App',
  components:{
    TheAbout,
  },
  data: function() {
    return {
      parentData: 'v-bind를 이용해 바인딩'
    }
  },
  }
</script>
```

- v-bind directive를 사용해 부모의 데이터의 props를 동적으로 바인딩
- 부모에서 데이터가 업데이트 될 때 마다 자식 데이터로도 전달
- data는 함수로 작성되어 새로운 객체를 반환 해야함

```vue
//자식.vue
<template>
<h2>{{ parentData }}</h2>
</template>

<scropt>
export default {
  name: 'TestCase',
  props: {
    myMessage: String,
    parentData: String,
  },
</scropt>
```



#### 숫자 전송시

```vue
<comp string-num="1"></comp>
<comp :int-num="1"></comp>
```

- 위의 방식은 "1"이라는 문자를 전달
- 아래 방식은 1이라는 숫자를 전달



### Emit event

- `$emit(eventName)`
  - 현재 인스턴스에서 이벤트를 트리거
- 부모 컴포넌트는 자식 컴포넌트가 사용되는 템플릿에서 v-on을 사용하여 자식 컴포넌트의 이벤트를 청취

```vue
<template>
  <div>
    <input 
    type="text"
    v-model="childInputData"
    @keyup.enter="childInputChange">
  </div>
</template>
<script>
export default {
  name: 'TestCase',
  props: {},
data : function(){return {}},
methods : {
  childInputChange: function(){
    this.$emit('child-input-change', this.childInputData)
  }
},
}
</script>
```

- `$emit`인스턴스 메서드를 사용해 'child-input-change' 이벤트를 트리거
  - `this.$emit('child-input-change', this.childInputData)`

```vue
<template>
  <div id="app">
    <the-about my-message="kebab-case"
    :parent-data="parentData"
    @child-input-change="parentGetChange"></the-about>
  </div>
</template>
<script>
import TheAbout from './components/TheAbout.vue'
export default {
  name: 'App',
  components:{TheAbout,},
  data: function() {return {}},
  methods: {
    parentGetChange: function(inputData) {
      console.log(`자식에게 ${inputData}받음`)
    }
  }
  }
</script>
```

- v-in directive를 사용하여 자식 컴포낸트가 보낸 이벤트 청취, method 실행
  - `@child-input-change`를 청취, `parentGetChange`실행



##### event 이름

- 자동 대소문자 변환을 제공x
  - kebab-case 사용 권장