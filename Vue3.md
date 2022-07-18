# Vue3와 Vue2의 문법상 차이
### 어플리케이션 인스턴스 생성
- Vue2
```Vue
import Vue from 'vue';
import App from './App.vue';

new Vue({
  render: h => h(App),
}).$mount('#app')
```

- Vue3
```Vue
import {createApp} from 'vue';
import App from './App.vue';
createApp(App).mount('#app');
```

### fragments
- 다중 루트 노드 컴포넌트인 fragments 지원
- template의 최상단에 div 태그를 이용하여 단일 태그로 안묶어도 됨

### Composition API
- 새로 추가
- 코드의 재사용성, 가독성 증가용
- data, methods 선언에 Composition API의 setup() 사용

- Vue2
```Vue
export default {
  data() {
    return {
      viewType: 'Vue2;,
    },
  },
  methods: {
    changeViewType (type) {
      ...
    },
  },
}
```

- Vue3
```Vue
export default {
  setup() {
    const state = reactive({
      viewType: 'Vue3',
    })

    const changeViewType (type){
      ...
    }
    return {
      state, changeViewType
    }
  }
}
```

### Lifecycle Hooks
- 접두어 `on`을 이용하여 컴포넌트의 라이프사이클 훅에 접근

- Vue2
```Vue
export default {
  beforeCreate() {
    ...
  },
  created() {
    ...
  },
}
```

- Vue3
```Vue
import { onBeforeMouint, onUpdated } from 'vue'

export default {
  setup () {
    onBeforeMount (()=>{
      ...
    })
    onUpdated (()=>{
      ...
    })
  }
}

#### Lifecycle 종류
- beforeCreate : setup()에서는 불필요
- created : setup()에서는 불필요
- beforeMount : `onBeforeMount`
- mounted : `onMounted`
- beforeUpdate : `onBeforeUpdate`
- updated : `onUpdated`
- beforeUnmount : `onBeforeUnmount`
- unmounted : `onUnmounted`
- errorCaptured : `onErrorCaptured`
- renderTracked : `onRenderTracked`
- renderTriggered : `onRenderTriggered`
  - Virtual Dom의 재 렌더링이 발생한 후 호출
- Activated : `onActvated`
- deactvated : `onDeactivated`

https://dltqhkoxgn1gx.cloudfront.net/img/posts/how-to-use-lifecycle-hooks-in-vue3-1.png

### props와 emit 분리
- setup()에서 전달인자로 명시하는 방식으로 분리

- Vue2
```Vue
//props 선언 & 사용
props: ['ljm'],
data: function(){
  return {
    lee: this.ljm
  }
}

//emit
methods : {
  childInputChange: function(){
    this.$emit('child-input-change', this.childInputData)
  }
},
```

-Vue3
```Vue
//props 선언 & 사용
export default {
  props: {
    lee: String
  },
  setup(props) {
    console.log(props.title)
  }
}

//emit
export default {
  setup(props,{ emit }) {
    ...
  }
}