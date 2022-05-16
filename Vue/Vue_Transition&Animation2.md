# Vue 트랜지션 & 애니메이션 - vol.2

## 엘리먼트 간 트랜지션

엘리먼트 간 트랜지션은 주로 목록 컨테이너와 빈 목록을 설명하는 메시지 사이에 사용된다. 

```vue
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Sorry, no items found.</p>
</transition>
```

### :warning:주의할 점

위의 경우와 달리 같은 태그 이름을 가진 엘리먼트 사이를 트랜지션할 때 , 해당 엘리먼트에 key 속성을 부여함으로써 별개의 엘리먼트임을 알려주어야 한다. 그렇지 않으면 Vue의 컴파일러는 효율성을 위해 엘리먼트의 내용만 바꾼다.

예제를 통해 한 번 살펴보자.

```vue
<template>
  <div>
    <transition name="fade-slide">
      <button v-if="isEditing" key="save" @click="changeIsEditing">Save</button>
      <button v-else key="edit" @click="changeIsEditing">Edit</button>
    </transition>
  </div>
</template>
<script>
export default {
  name: 'App',
  data: function () {
    return {
      isEditing: true,
    };
  },
  methods: {
    changeIsEditing() {
      this.isEditing = !this.isEditing;
    },
  },
};
</script>
<style>
.fade-slide-enter-active {
  transition: all 0.3s ease;
}
.fade-slide-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.fade-slide-enter, .fade-slide-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
...
<script>
export default {
  name: 'App',
  data: function () {
    return {
      isEditing: true,
    };
  },
  methods: {
    changeIsEditing() {
      this.isEditing = !this.isEditing;
    },
  },
};
</script>
<style>
.fade-slide-enter-active {
  transition: all 0.3s ease;
}
.fade-slide-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.fade-slide-enter, .fade-slide-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
```

예시를 보면 key 속성에 고유한 값을 부여해서 엘리먼트들을 구별할 수 있도록 했다. 만약 key 값이 없으면 트랜지션은 일어나지 않고 안의 값만 바뀌게 된다.

엘리먼트 간 트랜지션은 if else 구문 말고도 data를 활용해서도 구현할 수 있다.

```vue
<template>
  <div>
    <transition name="fade-slide">
      <button :key="isEditing" @click="changeIsEditing">
      {{ isEditing ? 'Save' : 'Edit' }}
      </button>
    </transition>
  </div>
</template>
```

여러 개의 엘리먼트 간 트랜지션은 switch case 구문을 사용하면 좋다.

```vue
<transition>
  <button v-bind:key="docState">
    {{ buttonMessage }}
  </button>
</transition>
...
computed: {
  buttonMessage: function () {
    switch (this.docState) {
      case 'saved': return 'Edit'
      case 'edited': return 'Save'
      case 'editing': return 'Cancel'
    }
  }
}
```

### 트랜지션 모드

엘리먼트 간 트랜지션이 일어날 때, 각 엘리먼트의 트랜지션은 순차적으로 일어나지 않는다. 두 엘리먼트의 트랜지션이 동시에 발생하는데, transition mode 속성에 특정 값을 추가해서 이를 해결할 수 있다.

- `in-out`: 처음에 새로운 엘리먼트가 트랜지션되고, 완료되면 현재 엘리먼트가 트랜지션된다.
- `out-in`: 현재 엘리먼트가 먼저 트랜지션되고, 완료되면 새로운 엘리먼트에 트랜지션이 일어난다.

## 컴포넌트간 트랜지션

컴포넌트간 트랜지션은 엘리먼트 간 트랜지션과 달리 key 속성을 부여줄 필요가 없다. 동적 컴포넌트를 사용하는 것으로 해당 문제를 해결할 수 있다.

## TransitionGroup

`<TransitionGroup>` 는 내장 컴포넌트로 리스트 형태로 렌더링되는 엘리먼트나 컴포넌트에 대해 삽입, 제거, 순서 바꾸기 등의 애니메이션을 적용시킬 때 사용한다.

- 기본적으로 wrapper 엘리먼트를 반환하지 않는다. tag 속성에 값을 정의해야 wrapper 엘리먼트를 반환한다.
- 양립할 수 없는 엘리먼트들 사이에서 전환이 일어나는 것이 아니기 때문에 트랜지션 모드는 적용할 수 없다.
- 엘리먼트들은 항상 고유한 key 속성을 가지고 있어야 한다.
- CSS 트랜지션 클래스들은 리스트의 각 엘리먼트에 적용되는 것이지, 그룹이나 컨테이너에 적용되는 것이 아니다.

예시를 통해 구체적인 사용 방법을 알아보자.

```vue
<template>
  <div>
    <transition-group name="fade" tag="ul">
      <li v-for="item in items" :key="item">
        {{ item }}
      </li>
    </transition-group>
    <button @click="pushNewNum">push</button>
    <button @click="removeNum">remove</button>
  </div>
</template>
<script>
export default {
  name: 'App',
  data: function () {
    return {
      items: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      nextNum: 10,
    };
  },
  methods: {
    pushNewNum() {
      this.items.push(this.nextNum);
      this.nextNum++;
    },
    removeNum() {
      this.items.pop();
      this.nextNum--;
    },
  },
};
</script>
<style>
.fade-enter-active {
  transition: all 0.5s ease;
}
.fade-leave-active {
  transition: all 0.5s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
```

## 트랜지션 재사용

```vue
// MyTransition.vue

<template>
  <Transition
    name="my-transition"
    @enter="onEnter"
    @leave="onLeave">
    <slot></slot>
  </Transition>
</template>

...

<style>
/*
	어차피 slot 컨텐츠에는 적용되지 않기 때문에
	scoped 사용을 자제하자.
*/
</style>
```

싱글 파일 컴포넌트와 같은 방법으로 만든 후 import 해서 사용하면 된다.

## 동적 트랜지션

트랜지션 컴포넌트의 name과 같은 속성은 동적으로 정의할 수 있다. 그래서 다양한 경우에 알맞는 트랜지션을 적용시켜줄 수 있다.

```vue
<Transition :name="transitionName">
  <!-- ... -->
</Transition>
```

# :books:참고자료

https://vuejs.org/guide/built-ins/transition.html