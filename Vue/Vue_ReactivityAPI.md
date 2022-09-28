# Vue

## Composition API

### Reactivity API : Core

### ref()

인자로 전달된 값을 받아 반응형 및 변동 가능한 `ref`객체를 반환한다. `ref`객체는 `.value`라는 단 하나의 프로퍼티를 가지고 있으며, 이 프로퍼티는 인자로 전달된 값을 가리킨다. 

### computed()

`getter` 함수를 전달받아 `getter`에서 반환된 값에 대한 읽기 전용인  `ref`객체를 반환한다. `get`과 `set` 함수가 있는 객체를 전달받아 `writable`한 `ref` 객체를 만들 수 있다. 여기서 `writable`이란 값을 정의할 수 있다라는 의미로 받아들이면 된다. 

#### readonly computed ref:

```js
const count = ref(1)
const plusOne = computed(() => count.value + 1)

console.log(plusOne.value) // 2

plusOne.value++ // error
```

위에서 말했듯이, `computed`는 `ref` 객체를 반환한다. 따라서 위의 예시에서 `plusOne`은 실제로는 `ref(2)`와 같은 것이다. 따라서 해당 값에 접근하려면 `value` 프로퍼티를 사용해야 한다.

#### writable computed ref:

```js
const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1
  }
})

plusOne.value = 1
console.log(count.value) // 0
```

`writable computed ref` 객체를 만들면서 `vue`가 아닌 `react`를 사용하는 듯한 느낌을 받았다. 아래는 `Composition API`를 사용하지 않았을 때, 동일한 결과를 출력하기 위한 코드이다.

```js
export default {
  data() {
    return {
      count: 1,
    }
  },
  computed: {
    plusOne: {
      get() {
        return this.count + 1
      },
      set(val) {
        this.count = val - 1
      },
    },
  },
  mounted() {
    this.plusOne = 1
    console.log(this.count) // 0
  },
}
```

기존에는 `vue`라는 틀에 맞추어 코드를 작성하는 느낌이었다면 이제는 `vue`를 활용해서 코드를 작성하는 느낌이 들었다. 

### reactive()

