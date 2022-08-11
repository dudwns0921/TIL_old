# Vue

##   *render: h => h(App)*

회사에서 작업을 하다가 위의 코드를 이해하지 못해 검색을 하게 되었다. 검색 후 해당 코드가 Vue 애플리케이션을 시작하는 코드인 걸 알게 되었다.

### Vue 시작하기

공식문서에 따르면 모든 Vue 애플리케이션은 `createApp` 함수를 사용해 새로운 애플리케이션 인스턴스를 만드는 것으로부터 시작된다. 그리고 mount 메서드를 사용하는 것으로 렌더링이 일어난다.

```js
import { createApp } from 'vue'

const app = createApp({
  /* root component options */
})
app.mount('#app')
```

Vue CLI를 통해 만들었던 App의 main.js에서 같은 코드가 있었기 때문에 회사 프로젝트 내에도 당연히 위 코드가 있을 거라고 생각했다. 막상 확인해보니 creatApp을 사용한 코드는 찾을 수 없었고, 아래의 코드만 찾을 수 있었다.

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
    el: '#app',
    render: h => h(App)
  })
```

조금은 낯선 형태의 코드이지만 위의 코드와 큰 차이는 없다. 

```js
el: '#app',
```

먼저 위의 코드부터 알아보자. el 옵션은 Vue 컴포넌트에서 제공하는 옵션으로 마운팅 포인트를 결정한다. 단 el 옵션은 Vue 생성자를 통해 인스턴스를 만들 때만 유효하다.

```js
render: h => h(App)
```

이 코드를 좀 더 알기 쉽게 바꾸면 다음과 같다.

```js
render: function(createElement) {
	return createElement(App);
}
```

내부적으로 `createElement`라는 함수를 파라미터로 받고 이 함수에 `App`을 넘겨줘서 `element`를 생성한다. `createElement`가 실제로 반환하는 것은 `VNode`이다.

`Vnode`란 모든 하위 노드의 설명과 페이지에 렌더링해야 하는 노드의 정보를 갖고 있는 객체이다. 이해하기가 어렵다면 `Virtual DOM`의 일부분이라고만 생각하자.

___

마지막으로 어떠한 과정을 거쳐서 위의 코드가 우리가 맨 처음 봤던 코드와 같이 바뀌었는지 알아보자.

##### 1. createElement는 단순히 변수명이기 때문에 `h`로 바꾼다.

```js
render: function(h) {
	return h(App);
}
```

createElement는 단순히 변수명이기 때문에 `h`로 바꿀 수 있다.

Vue의 창시자인 Evan You는 `h` 표현에 대해 다음과 같이 이야기했다.

> h는 hyperscript의 약자로 Virtual DOM에서 관용적으로 사용되는 표현인데, HTML 구조를 생성하는 스크립트라는 의미이다.

##### 2. 화살표 함수로 바꾼다.

```js
render: h => h(App)
```

좀 더 설명을 덧붙이자면 화살표 함수의 유일한 문장이 return이므로 return과 중괄호를 생략하면 맨 처음에 봤던 코드가 된다.

# :books:참고자료

https://vuejs.org/guide/essentials/application.html

https://goodteacher.tistory.com/85

https://handhand.tistory.com/251