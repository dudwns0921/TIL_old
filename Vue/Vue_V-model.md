# Vue

## V-model

프론트엔드에서 form 요소를 다루다보면 데이터를 서버에 전달하기 위해 form input의 state와 그와 상응하는 자바스크립트의 state를 동기화시켜야 하는 경우가 종종 있다. 아래 예시를 통해 그러한 경우를 좀 더 구체적으로 살펴보자.

```vue
<template>
  <form>
    <label for="id">아이디</label>
    <input
      id="id"
      type="text"
      :value="id"
      @input="
        event => {
          id = event.target.value;
        }
      "
    />
    <label for="password">비밀번호</label>
    <input
      id="password"
      type="password"
      :value="password"
      @input="
        event => {
          password = event.target.value;
        }
      "
    />
  </form>
</template>

<script>
export default {
  data() {
    return {
      id: '',
      password: '',
    };
  },
};
</script>

<style></style>

```

로그인 창을 만들어봤다면 위와 같은 코드에 익숙할 것이다. 전체적인 흐름을 먼저 살펴보자.

1. 사용자가 입력할 때마다 input 이벤트가 발생한다.
2. input 이벤트가 발생하면 아래 정의된 data 속성들의 값이 사용자가 입력한 값으로 변경된다.
3. input의 value는 각 data 속성에 바인딩되어  사용자의 입력에 따라 변경된 값이 즉각적으로 반영된다.

위 예시와 달리 회원가입 창처럼 훨씬 많은 input을 다룰 때 위와 같이 코드를 작성하는 건 꽤나 번거로운 일이다. **v-model**은 위의 코드를 아래와 같이 간단하게 바꾸어준다.

```vue
<template>
  <form>
    <label for="id">아이디</label>
    <input id="id" type="text" v-model="id" />
    <label for="password">비밀번호</label>
    <input id="password" type="password" v-model="password" />
  </form>
</template>

<script>
export default {
  data() {
    return {
      id: '',
      password: '',
    };
  },
};
</script>

<style></style>
```

**v-model**은 `textarea`, `select` 등 다양한 종류의 `input`들에 사용될 수 있다. **v-model**은 사용된 요소에 따라 자동으로 DOM 속성과 이벤트를 확장한다.

- text 타입의`input`과 `textarea` 요소에서는 `value` 속성과 `input` 이벤트를 사용한다.
- checkbox, radio 타입의 `input`에서는 `checked` 속성과 `change` 이벤트를 사용한다.
- `select` 엘리먼트는 `value` 속성과 `change` 이벤트를 사용한다.

## :bulb:Tip

v-model은 form 안의 요소들에서 확인되는 모든 속성들의 초기값을 무시한다. 

```vue
<input id="id" type="text" v-model="id" value="initialId" />
```

위의 예시에서 initialId의 값은 적용되지 않는다는 의미이다. v-model은 현재 바인딩된 데이터를 source of truth로 간주하기 때문에 초기값을 설정하고 싶다면 data 속성에 초기값을 선언해주면 된다.

```vue
<input id="id" type="text" v-model="id" value="initialId" />
...
<script>
export default {
  data() {
    return {
      id: 'initialId',
...
```



# :books:참고자료

https://vuejs.org/guide/essentials/forms.html