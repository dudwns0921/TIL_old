# Vue

## refs

`Vue`의 선언적 렌더링 모델은 대부분의 직접적인 `DOM` 작업을 추상화하지만, 여전히 `DOM` 요소에 직접 접근해야 하는 경우가 있을 수 있다. 이를 위해 `ref` 속성을 사용할 수 있다.

`ref`속성은 `DOM`이 **마운트된 후** 특정 `DOM`요소 또는 자식 컴포넌트 인스턴스에 직접 참조를 제공한다. `ref`속성은 아래와 같은 경우 유용하다.

- `input`요소에 프로그래밍적으로 `focus`이벤트를 적용시킬 때

## refs 접근하기

결과적으로 `DOM`요소 또는 자식 컴포넌트 인스턴스는 `this.$refs`에서 직접적으로 참조할 수 있다.

```vue
<script>
export default {
  mounted() {
    this.$refs.input.focus()
  }
}
</script>

<template>
  <input ref="input" />
</template>
```

앞에서도 이야기했지만 `ref`속성은 컴포넌트가 마운트된 이후, 다시 말하면 컴포넌트의 렌더링이 완료된 후에 접근이 가능하다는 점을 늘 기억하자.

# :books:참고자료

https://vuejs.org/guide/essentials/template-refs.html#accessing-the-refs