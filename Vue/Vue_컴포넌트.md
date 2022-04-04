# 🧩컴포넌트

컴포넌트는 화면의 영역을 구분하여 개발할 수 있는 뷰의 기능이다. 컴포넌트 기반으로 화면을 개발하게 되면 코드의 재사용성이 올라가고 빠르게 화면을 제작할 수 있다.

## 📄싱글 파일 컴포넌트

Vue에서 컴포넌트를 만들 때는 싱글 파일 컴포넌트라는 방식을 사용한다. 싱글 파일 컴포넌트는 화면의 특정 영역에 대한 HTML, CSS, JS 코드를 한 파일에서 관리하는 방법이다. 뷰 CLI로 프로젝트를 생성하고 나면 App.vue라는 파일을 확인할 수 있다. 이처럼 vue 확장자를 가진 파일을 모두 싱글 파일 컴포넌트라고 한다.

싱글 파일 컴포넌트는 뷰 로더에 의해 HTML, CSS, JS와 같은 웹 자원으로 분리되어 실행된다. [뷰 로더 (opens new window)](https://vue-loader.vuejs.org/guide/)는 웹팩의 로더 종류 중 하나이고 뷰 CLI로 프로젝트를 생성하면 기본적으로 설정이 되어 있다.

## 😎컴포넌트 사용해보기

기본적으로 App.vue라는 바탕이 되는 컴포넌트에 여러 컴포넌트를 부품처럼 끼워 넣어서 하나의 큰 프로젝트를 만든다고 생각하면 된다. 그러면 간단한 컴포넌트를 만들고 App.vue에서 해당 컴포넌트를 사용하는 방법에 대해서 알아보자.

```html
<!-- HeaderComponent.vue -->

<template>
  <>header</div>
</template>

<script>
export default {
  name: 'HeaderComponent',
};
</script>

<style></style>
```

vue 확장자를 가진 간단한 헤더 컴포넌트를 만들어보았다. 

```html
<!-- App.vue -->

<template>
  <div>Hi</div>
  <header-component></header-component>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue';

export default {
  name: 'App',
  components: {
    HeaderComponent,
  },
};
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

 App.vue에서 컴포넌트를 import하고 script 부분의 components에다가 해당 컴포넌트 이름을 입력해준다. 그리고 template에 해당 컴포넌트를 넣어주면 되는데, 위 코드에서 import한 컴포넌트와 template에 있는 컴포넌트의 이름이 다른 건 확인할 수 있을 것이다.

 Vue.js는 대소문자를 섞어 쓰는 PascalCase와 소문자 중간에 대시를 넣어 구분하는 kebab-case를 지원한다. 따라서 Todo로 등록한 컴포넌트는 todo로 사용 가능하고, 위의 경우에는 HeaderComponent 대신에 header-component로 사용 가능한 것이다.

## 🔃컴포넌트 통신

뷰 컴포넌트는 각각 고유한 데이터 유효 범위를 갖는다. 따라서, 컴포넌트 간에 데이터를 주고 받기 위해선 아래와 같은 규칙을 따라야 한다.

![뷰 컴포넌트 통신 방식](https://joshua1988.github.io/vue-camp/assets/img/component-communication.2bb1d838.png)

## Props

Props 속성은 컴포넌트 간에 데이터를 전달할 수 있는 컴포넌트 통신 방법이다. 프롭스 속성을 기억할 때는 상위 컴포넌트에서 하위 컴포넌트로 내려보내는 데이터 속성으로 기억하면 쉽다.

```html
<!--부모 컴포넌트-->

<template>
  <!-- 정적 데이터 전달 -->
  <ChildComponent message="안녕하세요! vue"></ChildComponent>
  <!-- 동적 데이터 전달 -->
  <ChildComponent v-bind:message="message"></ChildComponent>
</template>

<script>
import ChildComponent from './components/ChildComponent.vue';

export default {
  name: 'App',
  components: {
    ChildComponent,
  },
  data() {
    return {
      message: '안녕하세요! vue',
    };
  },
};
</script> },
};
</script>
```

```html
<!--자식 컴포넌트 ChildComponent.vue-->

<template>
  <h1>{{ message }}</h1>
</template>

<script>
export default {
  props: {
    message,
  },
};
</script>

<style></style>
```

부모 컴포넌트에서 정의된 message라는 데이터를 prop으로 ChildComponent로 보내주고 있다.

위 코드에서는 문자열을 전달하고 있지만 실제로 모든 타입의 변수가 prop으로 전달될 수 있다. 아래를 확인해보자.

```html
<ChildComponent v-bind:number="456"></ChildComponent>
```

위와 같이 v-bind를 통해 해당 값이 문자열이 아닌 Javascript 표현식임을 알려주고, Boolean, Array, Object 등 모든 타입을 전달할 수 있다.

## 이벤트 발생

이벤트 발생은 컴포넌트의 통신 방법 중 자식 컴포넌트에서 부모 컴포넌트로 통신하는 방식이다. 자식 컴포넌트에서 이벤트를 발신하고 부모에서 수신하는 간단한 예시를 한 번 만들어보겠다.

```html
<!--자식 컴포넌트-->

<template>
  <button @click="$emit('sendWarn')">누르면 알림 발생</button>
</template>

<script>
export default {};
</script>

<style></style>
```

```html
<!--부모 컴포넌트-->

<template>
  <child-component v-on:sendWarn="receiveWarn"></child-component>
</template>

<script>
import ChildComponent from './components/ChildComponent.vue';

export default {
  name: 'App',
  components: {
    ChildComponent,
  },
  data() {
    return {};
  },
  methods: {
    receiveWarn: function () {
      alert('자식 컴포넌트의 이벤트 받음');
    },
  },
};
</script>

<style>
</style>
```

먼저 전체적인 흐름을 살펴보자.

1. 자식 컴포넌트의 버튼을 누르면 부모 컴포넌트로 sendWarn이라는 이벤트를 발신한다.

2. 부모 컴포넌트에서 해당 이벤트를 수신하고 receiveWarn이라는 메서드를 실행시킨다.

```html
<button @click="$emit('sendWarn')">누르면 알림 발생</button>
```

각각의 과정을 코드를 보며 좀 더 자세히 살펴보자.

v-on 디렉티브를 통해 버튼에 클릭 이벤트가 발생할 때 sendWarn 이벤트를 발신한다. 위 코드에서는 button 태그 안에 인라인으로 작성했지만, 메서드에 따로 작성할 수도 있다.

```html
<template>
  <button v-on:click="sendWarn">누르면 알림 발생</button>
</template>

<script>
export default {
    methods: {
    sendWarn: function() {
        this.$emit('sendWarn');
}
}
};
</script>

<style></style>
```

 이와 같이 정리할 수도 있다. 개인적으로는 후자가 좀 더 깔끔한 코드라고 생각한다.

```html
<template>
  <child-component v-on:sendWarn="receiveWarn"></child-component>
</template>

<script>
import ChildComponent from './components/ChildComponent.vue';

export default {
  name: 'App',
  components: {
    ChildComponent,
  },
  data() {
    return {};
  },
  methods: {
    receiveWarn: function () {
      alert('자식 컴포넌트의 이벤트 받음');
    },
  },
};
</script>
```

부모 컴포넌트에서는 `v-on:자식 컴포넌트에서 발신한 이벤트명`을 입력해서 이벤트를 수신할 수 있다. 해당 디렉티브의 값으로 메서드나 연산을 할당해서 이벤트가 발생하면 작성한 메서드나 연산이 실행시킬 수 있다.

# :books:참고자료

[Props — Vue.js](https://kr.vuejs.org/v2/guide/components-props.html#Prop-%EC%9C%A0%ED%9A%A8%EC%84%B1-%EA%B2%80%EC%82%AC)

[Components | Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/components.html#%E1%84%8F%E1%85%A5%E1%86%B7%E1%84%91%E1%85%A9%E1%84%82%E1%85%A5%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC-%E1%84%8F%E1%85%A9%E1%84%83%E1%85%B3-%E1%84%92%E1%85%A7%E1%86%BC%E1%84%89%E1%85%B5%E1%86%A8)
