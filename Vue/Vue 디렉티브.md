# Vue 디렉티브

Vue에서 데이터와 화면을 연결하기 위해 가장 처음으로 사용했던 방법이 바로 머스태시 보간법이다. Vue에는 머스태시 보간법 외에도 데이터를 연결할 수 있는 또 다른 방법이 있는데, 바로 디렉티브이다.

> HTML 태그 안에 들어가는 하나의 속성으로 엘리먼트에게 동작을 지시하는 **지시문**이다.

둘의 차이를 간단한 예시를 통해 확인해보자.

```html
<!--머스태시 보간법-->
<template>
   <h1>{{message}}</h1>
 </template>
<!--디렉티브-->
<template>
  <h1 v-text="message"></h1>
</template>
```

```javascript
<script>
export default {
  name: 'App',
  components: {
  },
  // data: {
  //   message: "안녕하세요 vue!"
  // },
  data:()=>{
    return {
      message:"안녕하세요 vue!",
      }
    }
  }
</script>
```

v-text 디렉티브같은 경우 innerText와 동일한 기능을 수행하며 태그를 인코딩하여 문자 그대로 보여준다. 다만 머스태시 보간법과 달리 문자열과 섞어서 사용할 수 없기 때문에 머스태시 보간법을 더 많이 사용한다. 

이외에도 다양한 디렉티브들이 있는데, 하나씩 살펴보도록 하자.

## V-model

양방향 데이터를 공유할 때 사용하는 디렉티브이다.

```html
<template>
  <h1 v-text="message"></h1>
  <input type="text" v-model="message" />
</template>
```

인풋 값이 변경되면 message와 연결된 데이터들이 모두 실시간으로 업데이트된다. React의 state와 같이 상태 관리가 이루어진 것으로, 복잡한 자바스크립트 코드를 작성하지 않아도 간편하게 데이터를 업데이트할 수 있다. 

## V-html

innerHTML과 동일한 기능을 수행하며 태그를 파싱하여 화면에 구현한다. 좀 더 간단히 설명해보겠다. v-text는 문자 그대로를 보여주기 때문에 만약 message의 값을 `<h1>안녕하세요, vue!</h1>`라고 작성할 경우, 태그를 포함한 문자열이 그대로 나타난다. 태그를 적용하고 싶다면 v-html을 사용하면 된다.



# :books:참고자료

[디렉티브(Directives) | Vue.js](https://v3.ko.vuejs.org/api/directives.html#v-text)

[Vue.js의 기본 디렉티브 정리](https://uxgjs.tistory.com/112)
