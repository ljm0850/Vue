# syntax

- CDN
  - `<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>`

## Basic syntax

### Vue instance

```javascript
const ljm = new Vue({

})
```

- Vue 인스턴스 생서시 Option 객체를 전달해야 함
  - Option을 이용하여 동작 구현

- Vue Instance === Vue component



### Options

#### DOM - 'el'

```javascript
const ljm = new Vue({
    el: '#ljm'
})
```

- Vue 인스턴스와 DOM요소를 연결
- new를 이용한 인스턴스 생성 때만 사용



#### Data - 'data'

```javascript
const ljm = new Vue({
    el: '#ljm',
    data: {
        message: 'Vue start',
    },
})
```

- Vue 인스턴스의 데이터 객체
- Vue 인스턴스의 데이터를 정의

- v-bind, v-on과 같은 directive에서 사용 가능
- Vue 객체 내 다른 함수에서 **this** 키워드를 이용하여 접근 가능



#### Data - 'methods'

- Vue 인스턴스에 추가할 메서드

- v-in과 같은 directive에서도 사용 가능

- 객체 내 **this** 키워드를 통해 접근 가능

- **this**가 사용되는 경우 화살표 함수 사용시 this가 예상대로 작동하지 않음

  

## Template Syntax

- HTML 기반 템플릿 구문을 사용

- Interpolation
- Directive



### Interpolation(보간법)

- TEXT
  - `<sapn> 메시지 : {{ message }}</span>`

- Raw HTML
  - `<span v-html="message"></span>`

- Attributes
  - `<div v-bind:id="message"></div>`

- JS 표현식
  - {{ number + 1}}
  - {{ message.split('').reverse().join(' ') }}



### Directive(디렉티브)

- v-접두사가 있는 특수 속성
- 속성 값은 v-for를 제외하고는 단일 JS 표현식
- 표현식의 값이 변경될 때 반응적으로 DOM에 적용

- 전달인자(Arguments)
  - `:`를 통해 전달인자를 받음
- 수식어(Modifiers)
  - `.`으로 표시되는 특수 접미사

```html
<a v-bind:href="url"></a>
<form v-on:submit.prevent="onSubmit"></form>
```

#### v-text

```html
<div id = "ljm">
    <p v-text="message"></p>
    <p>{{ message }}</p>				<-- 두개가 동일 -->
</div>
<script>
    const app = new Vue({
        el: '#ljm',
        data : {
        message : 'v-text test',
    	}
  	})
</script>
```

- 엘리먼트의 textContent를 업데이트

#### v-html

```html
<div id="ljm">
    <div v-html="htmlTest"></div>
</div>
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          htmlTest: '<b>HTML 업데이트</b>',
      },
  })
</script>
```

- innerHTML을 업데이트
  - XSS 공격에 취약

#### v-show

```html
<div id="ljm">
    <p v-show="isTrue">true</p>
    <p v-show="isFalse">false</p>
</div>
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          isTrue : true,
          isFalse : false,
      },
  })
</script>
```

- 조건부 렌더링
- display CSS 속성을 토글 하는거일뿐
  - `<p>`태그는 존재하지만 공간을 차지 하지 않음(display: none)
  - ~~hidden~~ 
    - css에서 적용하는 hidden은 공간도 차지
  - 자주 변경되는 경우 v-if에 비해 렌더링이 덜 반복 되는 장점

#### v-if, v-else-if, v-else

```html
<div id="ljm">
    <div v-if="ifTest === 'T'">T</div>
    <div v-else-if="ifTest === 'F'">F</div>
    <div v-else>나도 몰라</div>
</div>
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          ifTest : 'T',
      }
  })
</script>
```

- 조건부 렌더링
- 조건에 해당할 경우에만 렌더링



#### v-for

```html
<div id="ljm">
    <div v-for="(food, index) in foods" :key="`food-${index}`">{{ food }}</div>
    <div v-for="todo in todos" :key="todo.id">
        {{ todo.title }} : {{ todo.check }}
    </div>
</div>
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          foods: ['냉면','콩국수','모밀국수'],
          todos: [
              { id: 1, title= '1일1알고', check: true },
              { id: 2, title= 'TIL', check: false },
              { id: 3, title= 'git', check: true },
          ],
      },
  })
</script>
```

- item 위치의 변수를 각 요소에서 사용할 수 있음
  - 객체의 경우 key

- v-for 사용시 반드시 **key 속성**을 각 요소에 작성

- v-if와 v-for는 동시에 사용하는것을 지양
  - 2.xx버전에서는 v-for가 우선순위
  - 3.xx버전에서는 v-if가 우선순위
  - 반복문에 문제 발생 할 수 있음



#### v-on

```html
<div id="ljm">
    <button v-on:click="alertTest">Button</button>
    <button @click="alertTest">Button</button>
</div>
<script>
  const app = new Vue({
      el: "#ljm",
      methods:{
      alertTest: function() {
          alert('테스트중입니다')
      },
  },
  })
</script>
```

- element에 addEventListener 적용
- 이벤트 유형은 전달 인자로 표시
- `@`를 이용하여 대체 가능



#### v-bind

```html
<div id="ljm">
    <img v-bind:src="imageSrc">
    <img :src="imageSrc">
    <hr>
    <div :class="{ active: isRed }">클래스 바인딩</div> <!-- active 조건 -->
    <div :class="[activeRed, myBackground ]">class에 두개가 생김</div>
</div>

<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          imageSrc: 'https://~~~~~',
          isRed = true,
          activeRed: 'active', // 라고 위에 할당해둔 class가 존재
          myBackground: 'my-background-color', // 라고 따로 할당해둔 class가 있음',
      }
  })
</script>
```

- HTML 요소의 속성에 Vue 상태 데이터를 값으로 할당
- Object 형태로 사용시 value가 ture인 key가 class 바인딩 값으로 할당

- `v-bind:href` === `:href`



#### v-model

```html
<div id="ljm">
  <h3>{{ myMessage }}</h3>
  <input v-model="myMessage" type="text">
</div>
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          myMessage: '',
      },
  })
</script>
```

- HTML form 요소의 값을 양방향 바인딩
- 수식어
  - `.lazy`: input 대신 change 이벤트 이후에 동기화
  - `.number`: 문자열을 숫자로 변경
  - `.trim`: 입력값에 대해 trim 진행



#### computed

```html
<script>
  const app = new Vue({
      el: '#ljm',
      data:{
          num: 1,
      },
      computed: {
          plusNum : function(){
              retrun this.num +1
          },
      },
  })
</script>
```

- 함수 형태로 정의하지만, 함수의 반환 값이 저장 & 바인딩됨
- **종속된 데이터가 변경될 떄만 함수를 실행**
- method와 비슷하지만 **값**을 요구할 때 주로 사용
- 선언형 프로그래밍
- 특정 데이터를 직접 사용/가공하여 다른 값으로 만들 떄 사용



#### watch

```html
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          num: 1,
      },
      watch: {
          num: function () {
              console.log(`${this.num}이 변경됨`)
          },
      },
  })
</script>
```

- 데이터를 감시하고, 데이터가 변할 시 실행되는 함수

- 특정 데이터의 변화 상황에 맞춰 다른 data가 바뀌어야 할 때 주로 사용
  - 콜백 함수 실행을 위한 트리거
- 명령형 프로그래밍



#### filter

```html
<div id="ljm">
    <p>{{ numbers | getOddNums | getUnderTenNums }}</p>
</div>
<script>
  const app = new Vue({
      el: '#ljm',
      data: {
          numbers: [1,2,3,4,5,6,7,8,9,10,11,12]
      },
      filters: {
          getOddNums: function (nums) {
              const oddNums = num.filter(function (num) {
                  return num %2
              })
              return oddNums
          },
          getUnderTenNums: function (nums) {
              const underTen = nums.filter(function (num){
                  return num <10
              })
              return underTen
          }
      }
  })
</script>
```

- 텍스트 형식화를 적용할 수 있는 필터
- interpolation , v-bind시 사용 가능
- `|` 를 이용하여 필터 역할