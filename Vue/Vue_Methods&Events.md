# Methods & Events

 자바스크립트의 function과 같은 기능을 사용하기 위해 Vue에서는 method를 사용한다. 간단한 카운터 예시를 한 번 만들어보자.

## Methods

```html
<template>
  <h1>{{count}}</h1>
  <button v-on:click="addNum">증가</button>
</template>
```

```javascript
<script>
export default {
  name: 'App',
  data:()=>{
    return {
      count:0,
      }
    },
  methods: {
    addNum(){
      this.count = this.count+1
    }
  }
  }
</script>
```

먼저 methods에 객체 형태로 함수를 정의한다. 그리고 해당 함수에서 count를 1씩 늘려주는 로직을 작성하면 되는데, 이 때 this 키워드를 꼭 사용해주어야 한다. 그래야만 data에 정의된 값들에 접근할 수 있게 된다.

또한 화살표 함수로 함수를 정의하면 안되는데, 그 이유는 화살표 함수는 this를 가질 수 없기 때문이다. 그래서 this 키워드를 사용하게 되면 다른 변수로 취급되고, 상위 객체인 Window 객체를 바인딩해 Uncaught TypeError 오류가 발생한다.

## Events

DOM과 관련된 이벤트가 발생하면 관련 정보는 모두 event 객체에 저장된다. 이벤트 객체의 데이터들을 다뤄보기 위해 마우스 위치를 알려주는 간단한 애플리케이션을 만들어보겠다.

```html
<template>
  <div :class="{windowDiv : true}" @click="getMousePosition($event)">
    <h1>{{message}}</h1>
  </div>
</template>
```

```javascript
<script>
export default {
  name: 'App',
  data:()=>{
    return {
      message:"",
      }
    },
  methods: {
    getMousePosition(event) {
      this.message = `마우스의 위치는 X : ${event.pageX} Y : ${event.pageY}`
    }
  }
  }
</script>
```

먼저 v-on은 @로 축약해서 사용할 수 있다. 그리고 클릭 이벤트 안에 있는 마우스 좌표 값을 얻기 위해 $event를 사용했다. 이 변수를 파라미터에 넣으면 메소드에서 이벤트에 접근할 수 있게 된다. 

# :books:참고자료

[이벤트 핸들링 — Vue.js](https://kr.vuejs.org/v2/guide/events.html)
