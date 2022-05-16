# Vue 트랜지션 & 애니메이션 - vol.3

## Class-based Animations

진입 진출 트랜지션을 사용하지 않고, class를 동적으로 적용시키는 것만으로도 애니메이션을 만들 수 있다.

```vue
<template>
  <div :class="{ shake: disabled }">
    <button @click="warnDisabled">Login</button>
    <span v-if="disabled">Check your ID again!!</span>
  </div>
</template>
<script>
export default {
  name: 'App',
  data: function () {
    return {
      disabled: false,
    };
  },
  methods: {
    warnDisabled() {
      this.disabled = true;
      setTimeout(() => {
        this.disabled = false;
      }, 1500);
    },
  },
};
</script>
<style scoped>
button {
  border-radius: 1rem;
  cursor: pointer;
}
span {
  margin-left: 1rem;
  color: red;
}
.shake {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
}
@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
</style>
```

로그인시 잘못된 정보가 입력되면 경고 문구와 함께 흔들리는 효과를 넣어봤다.

1. 버튼을 클릭하면 warnDisabled 메서드가 실행되어 disabled data 속성의 상태를 변경한다.
2. disabled가 true이면 shake 클래스가 적용되고 v-if에 의해 경고 문구가 나타난다.
3. warnDisabled에 의해 1.5초 이후에 다시  disabled의 상태가 변경되어 애니메이션과 경고 문구가 사라진다.

### animation

```
animation : duration | timing-function | delay | iteration-count | direction |
			fill-mode | play-state | name
```

animation은 총 8개의 animation 관련 속성들의 단축 속성으로 위와 같은 순서로 작성한다. 순서가 있기는 하지만 애초에 name 속성외에는 모두 키워드가 등록되어 있기 때문에 순서에 상관없이 적어도 작동하는 것 같다.

#### fill-mode

animation-fill-mode는 애니메이션 실행 이전이나 이후에 애니메이션 효과를 표시할지의 여부를 지정하는 속성이다. 애니메이션 실행 이전의 지연 시간에는 애니메이션에서 지정한 속성이 적용되지 않는데, 이 부분을 animation-fill-mode 속성을 통해 정의할 수 있다.

animation-fill-mode 속성 값으로 forwards를 지정한 경우에는 애니메이션의 마지막 키 프레임에 선언된 CSS 속성이 애니메이션 실행이 종료된 후에 표시되고, backwards를 지정한 경우에는 첫 번째 키프레임에 선언된 CSS 속성이 애니메이션이 시작하기 전이나 지연 시간에 표시된다. 이때 both 값을 지정하면 양쪽에 모두 적용된다.

### translate3d

translate3d() 메소드는 현재 위치에서 해당 요소를 주어진 x축과 y축, z축의 거리만큼 이동시킨다. 주어진 거리가 양수이면 해당 축의 양의 방향으로, 음수이면 해당 축의 음의 방향으로 이동시킨다. 위의 코드는 translateX 로도 작성할 수 있다.

## State-driven Animations

```vue
<template>
  <div
    @mousemove="onMouseMove"
    :style="{ backgroundColor: `hsl(${x}, 80%, 50%)` }"
    class="moveArea"
  >
    <p>Move your mouse across this div...</p>
    <p>x: {{ x }}</p>
  </div>
</template>
<script>
export default {
  name: 'App',
  data: function () {
    return {
      x: 0,
    };
  },
  methods: {
    onMouseMove(e) {
      this.x = e.clientX;
    },
  },
};
</script>
<style scoped>
.moveArea {
  border: 1px solid black;
  transition: 0.3s background-color ease-out;
}
</style>
```

1. mousemove 이벤트가 일어날 때마다 onMouseMove 메서드를 실행시킨다.
2. onMouseMove 메서드에 의해 data 속성의 x 값은 마우스 x 축 위치 값이 된다.
3. x 값은 hsl의 첫 번째 값인 색조에 적용되어 배경색을 바꾼다.

4. 트랜지션 속성에 의해 바뀐 배경색은 0.3초 후에 ease-out의 형태로 적용된다.



# :books:참고자료

https://vuejs.org/guide/extras/animation.html

https://developer.mozilla.org/ko/

https://greensock.com/