# vue

사용자 인터페이스를 만들기 위한 동적 javaScript 프레임워크    

<br/>

## 1. 특징
  * MVVM 패턴을 사용
  * Virtual DOM의 사용
  * Angular, React에 비해 매우 작고 가벼우며 복잡도가 낮음
  * Template과 Componenet를 사용하여 재사용이 가능한 사용자 인터페이스를 묶고 View Layer를 정리하여 사용
<br/>

## 2. MVVM 패턴
![image](https://github.com/yuuuuuuuuna/vue/assets/87286063/5721bf44-8747-4260-bc3d-bf6b78d65e6e)
   
   Mode - View - ViewModel의 줄임말으로 로직과 UI의 분리를 위해 설계된 패턴
   
   웹페이지는 돔과 자바스크립트로 만들어지게 되는데 돔이 View 역할을 하고, 자바스크립트가 Model 역할을 한다.
   
   뷰모델이 없는 경우에는 직접 모델과 뷰를 연결, 뷰모델이 중간에서 연결해 주는 것이 MVVM 모델

<br/>

## 3. vue life cycle
![333](https://github.com/yuuuuuuuuna/vue/assets/87286063/b5104b06-06eb-4477-8b27-86e757d83f6f)

### beforeCreate
가장 먼저 실행되고 컴포넌트 초기화, 아직 데이터와 이벤트가 설정되지 않은 단계
### created
템플릿 및 Virtual DOM이 마운팅 혹은 렌더링 되기 전에 실행, 데이터와 이벤트 접근 가능, 부모 > 자식 순으로 created
### beforeMount
DOM에 부착하기 직전에 호출, 가상 DOM이 생성되어 있으나 실제 DOM에 부착되지는 않은 상태
### mounted
가상 DOM의 내용이 실제 DOM에 부착되고 난 이후에 실행, this.$el을 비롯한 data, computed, methods, watch 등 모든 요소에 접근이 가능
* 자식 컴포넌트가 서버에서 비동기로 데이터를 받아오는 경우처럼, 부모의 mounted훅이 모든 자식 컴포넌트가 마운트 된 상태를 보장하지는 않는다. this.$nextTick을 이용
### beforeUpdate
컴포넌트에서 사용되는 data의 값이 변하기 직전에 호출
### updated
가상 DOM을 렌더링 하고 실제 DOM이 변경된 이후에 호출
### beforeDestroy
인스턴스가 해체되기 직전 호출
### destroyed
인스턴스가 해체되고 난 직후 호출

<br/>

## 4. Template 문법
### Interpolations
 * Double Mustaches
```
  <span>Message: {{ msg }}</span>
```

### Directives
v- 접두사를 가진 특별한 속성으로, HTML 요소에 특별한 반응형 동작을 적용


 * v-bind : HTML 속성과 Vue 데이터를 바인딩, 콜론(:)을 사용하여 v-bind 디렉티브를 단축 표기 가능
```
<a v-bind:href="url"> ... </a>
<a :href="url"> ... </a>
```
 * v-if, v-else, v-else-if : 조건부로 요소를 렌더링
```
<div v-if="isTrue">True content</div>
```
 * v-for : 배열이나 객체의 각 항목에 대해 요소를 반복
```
<li v-for="item in items" :key="item.id">{{ item.name }}</li>
```
 * v-on : 이벤트 리스너를 추가, @ 기호를 사용하여 v-on 디렉티브를 단축 표기 가능
```
<button v-on:click="doSomething">Click me</button>
<button @click="doSomething">Click me</button>
```
 * v-model : 양방향 데이터 바인딩을 구현
```
<input v-model="message" type="text">
```
 * v-show : 조건부로 요소를 표시하거나 숨깁
```
<div v-show="isShow">Show or hide content</div>
```

<br/>

## 5. Template Syntax
### method
함수를 단순히 정의
### computed
데이터 연산들을 정의
```
<template>
  <div>
    <p>원본 메시지: {{ message }}</p>
    <p>역순 메시지: {{ reversedMessage }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello Vue.js!',
    };
  },
  computed: {
    reversedMessage() {
      // 원본 메시지를 역순으로 변환하여 반환합니다.
      return this.message.split('').reverse().join('');
    },
  },
};
</script>
```
* computed 와 methods 의 차이점

computed : 대상 데이터의 값이 변경되면 자동으로 수행

methods : 호출할 때에만 해당 로직이 수행

methods 속성은 수행할 때마다 연산을 하기 때문에 별도로 캐싱을 하지 않지만, computed 속성은 데이터가 변경되지 않는 한 이전의 계산 값을 가지고 있다가 (캐싱하고 있다가) 필요할 때 바로 반환한다.
  
### watch
데이터 변화를 감지하여 자동으로 특정 로직을 수행
```
<template>
  <div>
    <input v-model="inputText" type="text" placeholder="Type something" />
    <p>입력된 텍스트 길이: {{ textLength }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputText: '',
      textLength: 0,
    };
  },
  watch: {
    inputText(newText) {
      this.textLength = newText.length;
    },
  },
};
</script>
```
* computed 와 watch 의 차이점
computed : 내장 API 를 활용한 간단한 연산 정도가 적합

watch : 데이터 호출 같이 시간이 상대적으로 더 많이 소모되는 비동기 처리에 적합

<br/>

## 6. 컴포넌트 데이터 전달
### props, emit
상위 -> 하위 props라는 속성을 전달
```
<!-- 상위 컴포넌트 -->
<template>
  <child-component :propName="data"></child-component>
</template>

<!-- 하위 컴포넌트 -->
<script>
export default {
  props: ['propName'],
  // ...
}
</script>
```

하위 -> 상위 이벤트만 전달 가능
```
<!-- 상위 컴포넌트 -->
<template>
  <child-component @customEvent="handleEvent"></child-component>
</template>

<script>
export default {
  methods: {
    handleEvent(data) {
    }
  }
  // ...
}
</script>

<!-- 하위 컴포넌트 -->
<template>
  <button @click="sendData">Send Data</button>
</template>

<script>
export default {
  methods: {
    sendData() {
      this.$emit('customEvent', data);
    }
  }
  // ...
}
</script>
```

### ref
마운트된 요소에만 적용가능

```
<!-- 상위 컴포넌트 -->
<template>
  <div>
    <child-component ref="childComponent"></child-component>
    <button @click="callChildMethod">Call Child Method</button>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent,
  },
  methods: {
    callChildMethod() {
      this.$refs.childComponent.childMethod();
    }
  }
};
</script>
```

```
<!-- 하위 컴포넌트 -->
<template>
  <div>
    <p>This is the child component</p>
  </div>
</template>

<script>
export default {
  methods: {
    childMethod() {
      console.log('Method called from the child component');
    }
  }
};
</script>
```
