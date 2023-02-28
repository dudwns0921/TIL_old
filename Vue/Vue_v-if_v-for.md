# Vue

## v-if와 v-for 함께 사용하기

공식문서는 v-if 디렉티브와 v-for 디렉티브를 같은 엘리먼트에 사용하는 걸 권장하지 않는다. 그 이유는 두 디렉티브가 같은 엘리먼트에 있을 경우, v-if가 v-for보다 우선순위가 높아 v-if의 조건에서 v-for의 스코프에 있는 변수에 접근할 수가 없기 때문이다. 

```vue
<!--
This will throw an error because property "todo"
is not defined on instance.
-->
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo.name }}
</li>
```

v-for 디렉티브를 template 태그에 옮기는 것으로 이 문제를 해결할 수 있다.

```
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```

template 태그는 화면에 불필요한 DOM을 생성하지 않는 특성을 가지고 있다.

# :books:참고자료

https://vuejs.org/guide/essentials/list.html#v-for-with-v-if