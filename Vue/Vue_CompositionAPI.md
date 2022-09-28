# Vue

## Composition API

**Composition API**는 옵션(`data`, `computed`, `watch` 등)을 선언하는 대신 `import`된 함수를 이용해 `Vue` 컴포넌트를 작성할 수 있도록 해주는 `API`들의 집합이다.

- ### **Reactivity API**

  #### 반응형 `state`, `computed state`, 그리고 `watcher`를 만들 수 있다.

  - `ref()`
  - `reactive()`

- ### **Lifecycle Hooks**

  #### 컴포넌트 라이프 사이클에 프로그래밍 방식으로 연결할 수 있다.

  - `onMounted()`
  - `onUnmounted()` ...

- ### **Dependency Injection**

  #### `Reactivity API`를 사용할 때 `Vue`의 의존성 주입 시스템을 활용할 수 있도록 한다.

  - `provide()`
  - `inject()`...

`Composition API`는 `Vue 3`과 `Vue 2.7`의 내장되어있다. 더 오래된 Vue 2 버전에서는 `@vue/composition-api plugin`을 사용하면 된다. `Vue 3`에서의 구체적인 사용 방법은 아래 예시를 참고하자.

```vue
<script setup>
import { ref, onMounted } from 'vue'

// reactive state
const count = ref(0)

// functions that mutate state and trigger updates
function increment() {
  count.value++
}

// lifecycle hooks
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

## 장점

- 더 나은 로직 재사용
- 더 유연한 코드 구성
- 더 나은 타입 추론

# :books:참고자료

https://vuejs.org/guide/extras/composition-api-faq.html#what-is-composition-api