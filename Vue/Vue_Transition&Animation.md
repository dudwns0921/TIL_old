# 트랜지션 & 애니메이션 - vol.1

## 개요

Vue는 항목이 DOM에 삽입, 갱신 또는 제거 될 때 트랜지션 효과를 적용하는 다양한 방법을 제공한다. 여기에는 다음과 같은 도구가 포함된다.

- CSS 트랜지션 및 애니메이션을 위한 클래스를 자동으로 적용됨.
- Animate.css와 같은 타사 CSS 애니메이션 라이브러리 통합
- 트랜지션 훅 중에 JavaScript를 사용하여 DOM을 직접 조작
- Velocity.js와 같은 써드파티 JavaScript 애니메이션 라이브러리 통합

## 단일 엘리먼트 / 컴포넌트 트랜지션

Vue는 transition wrapper 컴포넌트를 제공하므로 다음과 같은 상황에서 모든 엘리먼트 또는 컴포넌트에 대한 진입 / 진출 트랜지션을 추가할 수 있다.

- 조건부 렌더링 (`v-if` 사용)
- 조건부 출력 (`v-show` 사용)
- 동적 컴포넌트
- 컴포넌트 루트 노드

다음 매우 간단한 예제를 보자.

```vue
<template>
  <div>
    <button @click="show = !show">Toggle</button>
    <transition name="fade">
      <p v-if="show">hello</p>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      show: true,
    };
  },
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
```

트랜지션 또는 애니메이션은 `transition` 컴포넌트로 싸여진 엘리먼트가 삽입되거나 제거될 때 일어난다:

1. Vue는 대상 엘리먼트에 CSS 트랜지션 또는 애니메이션이 적용되었는지 여부를 자동으로 감지한다. 만약 그렇다면, CSS 트랜지션 클래스가 적절한 타이밍에 추가 혹은 제거된다.
2. 트랜지션 컴포넌트가 [JavaScript 훅](https://kr.vuejs.org/v2/guide/transitions.html#JavaScript-Hooks)를 제공하면 이러한 훅은 적절한 타이밍에 호출된다.
3. CSS 트랜지션 / 애니메이션이 감지되지 않고 JavaScript 훅이 제공 되지 않으면 삽입 또는 제거를 위한 DOM 작업이 다음 프레임에서 즉시 실행된다. 

### 트랜지션 클래스

1. `v-enter`: enter의 시작 상태. 엘리먼트가 삽입되기 전에 적용되고 한 프레임 후에 제거된다.
2. `v-enter-active`: enter에 대한 활성 및 종료 상태. 엘리먼트가 삽입되기 전에 적용된다. 트랜지션 / 애니메이션이 완료되면 제거된다.
3. `v-enter-to`: **2.1.8 이상 버전에서 지원한다.** 진입 상태의 끝에서 실행된다. 엘리먼트가 삽입된 후 (동시에 `v-enter`가 제거됨), 트랜지션/애니메이션이 끝나면 제거되는 하나의 프레임을 추가한다.
4. `v-leave`: leave를 위한 시작 상태. 진출 트랜지션이 트리거 될 때 적용되고 한 프레임 후에 제거된다.
5. `v-leave-active`: leave에 대한 활성 및 종료 상태. 진출 트랜지션이 트리거되면 적용되고 트랜지션 / 애니메이션이 완료되면 제거된다.
6. `v-leave-to`: **2.1.8 이상 버전에서 지원한다.** 진출 상태의 끝에서 실행된다. 진출 트랜지션이 트리거되고 (동시에 `v-leave`가 제거됨), 트랜지션/애니메이션이 끝나면 제거되는 하나의 프레임을 추가한다.

위 클래스들은 자동으로 생성되는 클래스들로 접두어인 v는 transition 컴포넌트의 name prop의 값에 따라 달라진다. 가령 위의 예시에서는 fade로 이름을 붙였기 때문에 fade-enter와 같이 클래스 이름들이 작성된다.

![Transition Diagram](./md-images/transition.png)

각 클래스들의 내용이 조금 어렵게 느껴질 수 있는데, 시작 - 진행 - 종료 정도의 개념으로 이해해도 충분하다고 생각한다. enter 혹은 leave에서는 시작 상태를, -active에서는 트랜지션 또는 애니메이션을 정의, 마지막으로 -to 에서는 끝날 때의 상태를 정의한다고 생각하자.

### CSS 트랜지션

가장 일반적인 유형의 트랜지션은 CSS 트랜지션을 사용하는 것이다. 아까의 예시에서 transition 컴포넌트의 이름만 slide-fade로 바꾸었다. 

```css
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
```

## :bulb:Tip

###  cubic-bezier

cubic-bezier()는 cubic Bezier curve를 정의하는 함수적 표기법이다. cubic Bezier curve는 트랜지션의 시작과 끝을 부드럽게 하는데 종종 사용되며 이에 따라 easing function이라고 불리기도 한다. 

cubic Bézier curve는 P0, P1, P2, 그리고 P3의 총 네 가지 지점에 의해 정의된다. P0과 P3는 곡선의 시작과 끝으로, P0은 (0, 0), P3은 (1,1)의 고정된 값을 갖고 있다. P0은 초기 시간 또는 위치 및 초기 상태를 나타내며, P3는 최종 시간 또는 위치 및 최종 상태를 나타낸다. 

P0과 P3가 고정된 값을 갖고 있기 때문에 P1과 P2의 값은 그 안의 값을 가져야만 한다. 그렇지 않을 경우 CSS는 해당 속성을 무시해버린다.

### 사용방법

```css
cubic-bezier(x1, y1, x2, y2)
```

cubic-bezier에는 P1, P2의 좌표값을 정의하면 된다. 언급한 바 있지만 P1과 P2는 P0과 P3 사이의 좌표값을 가져야만 한다. 많이 쓰이는 좌표값들은 다음과 같이 키워드로 미리 정의해놓았다.

- `ease` : cubic-bezier(0.25, 0.1, 0.25, 1.0)
- `ease-in` : cubic-bezier(0.42, 0.0, 1.0, 1.0)
- `ease-in-out` : cubic-bezier(0.42, 0.0, 0.58, 1.0)
- `ease-out` : cubic-bezier(0.0, 0.0, 0.58, 1.0)

### CSS 애니메이션

CSS 애니메이션은 CSS 트랜지션과 동일한 방식으로 적용된다. 다만 차이점이 있다면 요소가 삽입된 이후에 v-enter가 제거되지 않고, animationend 이벤트에서 제거된다. 

### 사용자 지정 트랜지션 클래스

transition 컴포넌트는 앞에서 살펴본 클래스들을 속성으로 가진다. 해당 속성에 새로운 클래스명을 정의함으로써 클래스명을 오버라이드할 수 있다. 이는 Vue의 트랜지션 시스템을 [Animate.css](https://daneden.github.io/animate.css/)와 같은 기존 CSS 애니메이션 라이브러리와 결합하려는 경우 특히 유용하다.

```vue
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
```

### 트랜지션 지속 시간 명시

대부분의 경우 Vue는 transitionend 또는 animationend 이벤트를 통해 트랜지션의 완료되는 걸 자동으로 감지한다. 하지만 duration 속성을 사용해서 트랜지션 지속 시간을 명시할 수도 있다.

```vue
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

### Javascript 훅

transition 컴포넌트의 이벤트에 대해 핸들러를 추가해주는 걸로도 트랜지션을 구현할 수도 있다.

```vue
<Transition
  @before-enter="onBeforeEnter"
  @enter="onEnter"
  @after-enter="onAfterEnter"
  @enter-cancelled="onEnterCancelled"
  @before-leave="onBeforeLeave"
  @leave="onLeave"
  @after-leave="onAfterLeave"
  @leave-cancelled="onLeaveCancelled"
>
  <!-- ... -->
</Transition>
```

```javascript
export default {
  // ...
  methods: {
    // 엘리먼트가 DOM에 삽입되기 전에 실행된다.
    // 진입 당시의 엘리먼트의 상태를 정의하기 위해 사용한다.
    onBeforeEnter(el) {},

    // 엘리먼트가 삽입되고 1 프레임 이후에 실행된다.
    // 애니메이션을 시작할 때 사용한다.
    onEnter(el, done) {
      // done 콜백 함수를 호출함으로써 트랜지션이 종료되었음을 알릴 수 있다.
      done()
    },

    // 진입 트랜지션이 끝났을 때 실행된다.
    onAfterEnter(el) {},
    onEnterCancelled(el) {},

    // 진출 hook 전에 실행된다.
    // 대부분의 경우 leave hook만을 사용한다.
    onBeforeLeave(el) {},

    // 진출 트랜지션 혹은 애니메이션이 시작될 때 실행된다.
    onLeave(el, done) {
      done()
    },

    // 진출 트랜지션이 끝나고 엘리먼트가 DOM에서 제거될 때 실행된다.
    onAfterLeave(el) {},

    // v-show 트랜지션하고만 사용가능하다.
    leaveCancelled(el) {}
  }
}
```



# :books:참고자료

https://vuejs.org/guide/built-ins/transition.html#the-transition-component

https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function