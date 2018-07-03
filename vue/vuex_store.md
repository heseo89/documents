# Vuex
Vue.js 의 상태관리 를 위한 패턴이자 라이브러리. 다른 상태관리 패턴이나 라이브러리와 비교했을 때 Vue 의 Reactivity 체계를 효율적으로 활용하여 화면 업데이트가 가능하다는 차이점이 있다.

## Vuex와 State.

### 상태관리 (State Management) 가 왜 필요한가?
* 컴포넌트 기반 프레임워크에서는 화면 구성을 위해 화면 단위를 매우 잘게 쪼개서 컴포넌트로 사용한다. 예를 들면, header, button, list 등의 작은 단위들이 컴포넌트가 되어 한 화면에서 많은 컴포넌트를 사용하게 된다. 이에 따라 컴포넌트 간의 통신이나 데이터 전달을 좀 더 유기적으로 관리할 필요성이 생긴다.
달리 말해, header -> button, button -> list , button -> footer 등의 컴포넌트 간 데이터 전달 및 이벤트 통신 등의 여러 컴포넌트의 관계를 한 곳에서 관리하기 쉽게 구조화 하는 것이 State Management다.
Vue 와 성격이 유사한 프론트엔드 프레임워크인 React 에서는 이미 Redux, Flux 와 같은 상태 라이브러리를 사용하고 있고, Vue 도 Vuex 라는 상태관리 라이브러리를 사용한다.

### 상태관리로 해결할 수 있는 문제점?
* 상태관리는 중대형 규모의 앱 컴포넌트들을 더 효율적으로 관리하기 위한 기법이다. 일반적으로 앱의 규모가 커지면서 생기는 문제점들은 아래와 같다.
Vue 의 기본 컴포넌트 통신방식인 상위 - 하위 에서 중간에 거쳐야 할 컴포넌트가 많아지거나
이를 피하기 위해 Event Bus 를 활용하여 상하위 관계가 아닌 컴포넌트 간 통신 시 관리가 되지 않는 점
이러한 문제점들을 해결하기 위해 모든 데이터 통신 (state) 을 한 곳에서 중앙 집중식으로 관리한다

* 상태관리 패턴
상태관리 구성요소는 크게 3가지가 있다.
```
state : 컴포넌트 간 공유될 data
view : 데이터가 표현될 template
actions : 사용자의 입력에 따라 반응할 methods
```

##### getters
1. 중앙 데이터 관리식 구조에서 발생하는 문제점 중 하나는 각 컴포넌트에서 Vuex의 데이터를 접근할 때 중복된 코드를 반복호출하게 되는 것.
```
<!-- App.vue -->
<div id="app">
  Parent counter : {{ parentCounter }}
  <!-- ... -->
</div>

<!-- Child.vue -->
<div>
  Child counter : {{ childCounter }}
  <!-- ... -->
</div>
```

```
// App.vue
computed: {
  parentCounter() {
    return this.$store.state.counter;
  }
},

// Child.vue
computed: {
  childCounter() {
    return this.$store.state.counter;
  }
},
```
2. Getters 등록
```
export const store = new Vuex.Store({
  // ...
  getters: {
    getCounter: function (state) {
      return state.counter;
    }
  }
});
```
3. Getters 사용
```
// App.vue
computed: {
  parentCounter() {
    this.$store.getters.getCounter;
  }
},

// Child.vue
computed: {
  childCounter() {
    this.$store.getters.getCounter;
  }
},
```

##### mutation
1. vuex의 데이터, 즉 state 값을 변경하는 로직을 의미.  getters와 차이점은 인자를 받아 vuex에 넘겨 줄 수 있고,  computed가 아닌 methods에 등록함.
2. 동기적 로직을 정의 (순차적)
3.  State 관리에 주안점을 두고 있다. 상태관리 자체가 한 데이터에 대해 여러 개의 컴포넌트가 관여하는 것을 효율적으로 관리하기 위함인데 Mutations 에 비동기 처리 로직들이 포함되면 같은 값에 대해 여러 개의 컴포넌트에서 변경을 요청했을 때, 그 변경 순서 파악이 어렵기 때문이다.

##### actions
1. 비 순차적 또는 비동기 처리 로직들을 선언한다.
2. 결과를 받아올 타이밍이 예측되지 않은 로직을 보통 actions에 선언.
3. 상태가 변화하는 걸 추적하기 위해 actions는 결국 mutations의 메서드를 호출(commit)하는 구조가 됨.
    http get요청이나 settimeout 과 같은 비동기 처리 로직들을 보통 처리
4. actions를 호출할 때는 아래와 같이 dispatch()를 이용함.
```
// App.vue 또는 components/$.vue
methods : {
  // Mutations를 이용할 때 (commit 사용)
  addCounter() {
    this.$store.commit('addCounter')
  }
  // Actions를 이용할 때 (dispatch 사용)
  addCounter() {
    this.$store.dispatch('addCounter')
  }
}
```

##### mapGetters, mapMutations, mapActions
1. mapGetters
```
<!-- App.vue -->
<div id="app">
  Parent counter : {{ parentCounter }}
  <!-- ... -->
</div>

```

```
// App.vue
import { mapGetters } from 'vuex'

// ...
computed: mapGetters({
  parentCounter : 'getCounter' // getCounter 는 Vuex 의 getters 에 선언된 속성 이름
}),
```
```
// App.vue
import { mapGetters } from 'vuex'

computed: {
  ...mapGetters([
    'getCounter'
  ]),
  anotherCounter() {
    // ...
  }
}
```

2. mapMutations
```
// App.vue
import { mapMutations } from 'vuex'

methods: {
  // Vuex 의 Mutations 메서드 명과 App.vue 메서드 명이 동일할 때 [] 사용
  ...mapMutations([
    'addCounter'
  ]),
  // Vuex 의 Mutations 메서드 명과 App.vue 메서드 명을 다르게 매칭할 때 {} 사용
  ...mapMutations({
    addCounter: 'addCounter' // 앞 addCounter 는 해당 컴포넌트의 메서드를, 뒤 addCounter 는 Vuex 의 Mutations 를 의미
  })
}
```

3. mapActions
```
import {mapActions} from 'vuex';

export default {
  methods: {
    ...mapActions([
      'asyncIncrement',
      'asyncDecrement'
    ])
  },
}

```

Link : [Google](https://google.com)
*single asterisks*
_single underscores_
**double asterisks**
__double underscores__
++underline++
~~cancelline~~
