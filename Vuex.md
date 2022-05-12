# Vuex

## Vuex

- 상태를 전역 저장소로 관리할 수 있도록 지원하는 라이브러리
  - 애플리케이션의 모든 컴포넌트에 대한 **중앙 집중식 저장소**



### State

- state = data, 애플리케이션의 핵심이 되는 요소
- 중앙에서 관리하는 모든 상태 정보



### 상태 관리 패턴

- 컴포넌트의 공유된 상태를 추출, 전역에서 관리
- 트리에 상관 없이 상태에 엑세스, 동작을 트리거 가능
  - 부모-자식-손자 컴포넌트에서 부모-손자로도 가능

#### 기존 Pass props & Emit event

- 각 컴포넌트는 독립적으로 데이터 관리
- 단뱡항 부모->자식으로만 전달, 반대의 경우 이벤트 트리거 이용하여 전달
  - 동위 관계 및 다른 컴포넌트로 데이터 전달이 불편



### Vuex management pattern

- 중앙 저장소(store)에 state를 모아두고 관리
- 규모가 큰 프로젝트에서 효율적
- 각 컴포넌트에서는 중앙 집중 저장소의 state만 신경씀



## Vuex Core Concepts

### State

- 중앙에서 관리하는 모든 상태 정보 (data)
  - single state tree 사용
  - 원본 소스의 역할
- 여러 컴포넌트 내부에 있는 state를 중앙에서 관리

- State가 변화시 해당 state를 공유하는 컴포넌트들이 DOM을 렌더링



### Mutations

- 실제로 state를 변경하는 유일한 방법
- 핸들러함수(handler)는 반드시 동기적
  - 비동기적 로직(콜백...)은 state가 변화 하는 시점이 달라질 수 있음
- 첫번째 인자로 항상 **state**를 받음
- Action에서 **commit()** 메서드에 의해 호출



### Actions

- mutations를 **commit()** 메서드로 호출해서 실행
- 비동기 작업이 포함될 수 있음
- **context**객체 인자를 받음
  - context 객체를 통해 모든 요소의 속성 접근, 메서드 호출 가능
  - state 변경이 가능은 하지만, 역할 분담(통일성)을 위해 하지는 않음
- 컴포넌트에서 dispatch() 메서드에 의해 호출됨



### Getters

- state를 변경하지 않고, 활용하여 계산을 수행(computed 와 유사)

  - 계산된 값만 가져옴

- getters의 결과는 state 종속성에 따라 캐시(cashed)되고, 종속성이 변경된 경우 재계산됨

  

----

## Vuex 활용

- 프로젝트 생성
  - `vue create appname`
  - `cd appname`
  - `vue add vuex`
    - vue에 vuex 추가
    - store 디렉토리 생성, 그 안에 index.js 생성(Vuex core concepts가 작성되는 곳)



### State

#### index.js

```javascript
// index.js
export default new Vuex.Store({
	state: {
		todos:[{title: 'vuex', date: new Date().getTime()},
               {title: '실습', date: new Date().getTime()}],
}
})
```

- object로 state에 추가

#### .vue

```javascript
// .vue
<template>
    <div>
    	<childApp v-for="todo in $store.state.todos" :key="todo.date" />
        <childApp v-for="todo in todos" :key="todo.date" />		//computed에 작성시
            
	~~~~
            
<script>            
export default {
	computed: {
        todos: function() {
            return this.$store.state.todos
        }
    }
}
```

- `$store.state`를 통해 .vue파일에서 저장소의 state에 접근

- HTML 부분 간소화를 위해 computed에 작성하여 자주 사용



### Actions & Mutations

```javascript
// index.js
export default new Vuex.Store({
    state: { todosd:[],},
    mutations: {
        CREATE_TODO: function (state, todoItem) {
            state.todos.push(todoItem)
        }
    },
    actions: {
        createTodo: function (context, todoItem) {
            context.commit('CREATE_TODO', todoItem)
        }
    }
})
```

#### Actions

- createTodo 함수를 만들어 createTodo에서 CREATE_TODO 호출
- **context**
  - Vuex store의 전반적인 속성을 모두 포함(만능)
  - context.state , context.getters, ...
  - actions의 함수 첫  인자로 context 대신 getters 등을 가져오기도 함
    - toDos({state}) {~~~} ...

#### Mutations

- CREATE_TODO 함수로 State의 todo 데이터 조작
- 함수 이름을 대문자로 주로 작성(디버깅 하기에 편해서)



```javascript
// .vue
<script>
    export default {
	methods: {
        createTodo: function () {
            const todoItem = {
                title: this.todoTitle,
                date: new Date().getTime(),
            }
            if (todoItem.title) {
                this.$store.dispatch('createTodo', todoItem)
            }
        }
    }
}
```

- `$store.dispatch('Actions name', instance)` 형태로 함수 호출



#### CRUD

- Create : push

- Delete

  - indexOf를 사용하여 index 번호 확인

  - splice(index, 지울 개수) : index 번 부터 차례로 지우고, 지운 배열을 반환

  - ```javascript
    // index.js
    mutations: {
        DELETE_TODO: function (state, todoItem) {
            const index = state.todos.indexOf(todoItem)
            state.todos.splice(index, 1)
        }
    }
    ```

- Update

  - ```javascript
    // index.js
    
    mutations: {
        UPDATE_TODO_STATUS: function (state, todoItem) {
            state.todos = state.todos.map(todo => {
                if (todo === todoItem) {
                    return {
                        title: todoItem.title,
                        date: new Date().getTime(),
                    }
                } else {
                    return todo
                }
            })
        }
    }
    ```

    - 선택된 todoItem과 현재 todos의 요소 todo가 일치하면 갱신

  - `:class="{'is-completed': todo.isCompleted}" ` 형식으로 html에서 클래스에 is-completed를 추가 가능(todo.isCompleted에 따라 on//off)

#### Javascript Spread Syntax

- 객체 복사시에 주로 사용 ()

```javascript
const todoItem = {
    todo: 'vuex 알아보기',
    isCompleted: false,
}
// isCompleted 변경해보기1
const myUpdateTodo = {
    todo: 'vuex 알아보기',
    isCompleted: true,
}
// isCompleted 변경해보기2
const myUpdateTodo2 = {
    ...todoItem,
    isCompleted: ture,
}
```

- 이를 이용하여 Update에서 `title: todoItem.title,` 를 `...todo`로 변경하고 바꿀 부분만 입력하여 update 가능



### Getters

```javascript
// index.js 완료된 todo 개수 계산
getters: {
    completedTodosCount: function (state) {
        return state.todos.filter(todo => {
            return todo.isCompleted === true
        }).length
    }
}
```

```javascript
// .vue
computed: {
    com~~: function () {
        return this.$store.getters.completedTodosCount
    }
}
```

- `$store.getters`를 이용하여 불러옴
- computed 반환값으로 사용



## Component Binding Helper

### Component Binding Helper

- 배열 조작을 쉽게 하기 위해 사용
- 종류
  - mapState
  - mapGetters
  - mapActions
  - mapMutations
  - ~~createNamespacedHelpers~~



#### mapState

- computed와 Store의 state를 매핑
- 매핑된 computed 이름과 state의 이름이 같을 때 가능
- mapState()는 객체를 반환

```javascript
// 기존
computed: {
    todos: function () {
        return this.$store.state.todos
    }
}
```

```javascript
// mapState
import { mapState } from 'vuex'
computed: mapState([
    'todos',
])
```

- 다른 computed 값과 함께 사용할 수 없어 Spread를 이용하여 주로 사용

```javascript
computed: {
    ...mapState([
        'todos'
    ])
}
```



#### mapGetters

- Computed와 Getters를 매핑
- 마찬가지로 Spread를 이용하여 추가

```javascript
import { mapGetters } from 'vuex'
computed: {
    ...mapGetters([
        'getterOne',
        'getterTwo'
    ])
}
```



#### mapActions

- action을 전달하는 컴포넌트, method 옵션을 만들 때 사용
- 마찬가지로 Spread를 이용하여 추가
- dispatch()를 사용 했을 때는 methods에서 인자를 넘겨줌
  - mapActions 사용시에는 template에서 인자를 넘겨줌(pass prop)



## LocalStorage

- `$ npm i vuex-persistedstate`
  - Vuex state를 자동으로 브라우저의 LocalStorage에 저장해주는 라이브러리
  - 페이지가 새로고침 되어도 Vuex state를 유지

```javascript
// index.js
import createPersistedState from 'vuex-persistedstate'

export default new Vuex.Store({
    plugins: [
        createPersistedState(),
    ]
})
...
// ------------
actions: {
    saveTodos({ state }) {
        const jsonData = JSON.stringify(state.todos)
        localStorage.setItem('todos', jsonDAta)
    }
}
```

- ~~저장이 필요한 함수 실행시마다 `dispatch('saveTodos')`를 이용하여 저장~~
- 불러오기는 `localStorage.getItem('name')`
- 자세한건 공식 문서를 찾아보자
