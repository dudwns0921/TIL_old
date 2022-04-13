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

이외에도 다양한 기능을 가진 디렉티브들이 있는데, 하나씩 살펴보도록 하자.

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

innerHTML과 동일한 기능을 수행하며 태그를 파싱하여 화면에 구현한다.

좀 더 풀어서 설명하자면 v-text는 문자 그대로를 보여주기 때문에 만약 message의 값을 `<h1>안녕하세요, vue!</h1>`라고 작성할 경우, 태그를 포함한 문자열이 그대로 나타난다. 태그를 적용시킨 문자를 보여주고 싶다면 v-html을 사용하면 된다.

## V-bind

태그 사이의 값이 아닌 태그의 속성 값을 동적으로 변경할 때 사용하는 디렉티브이다.

다양하게 사용할 수 있으며 v-bind:가 아니라 :으로도 대체할 수 있다.

```html
<!-- 속성 바인드 -->
<img v-bind:src="imageSc" />

<!-- 동적 속성 이름 -->
<button v-bind:[key]="value"></button>

<!-- 약어 -->
<img :src="imageSrc" />

<!-- 약어를 적용한 동적 속성 이름 -->
<button :[key]="value"></button>

<!-- 인라인 문자열 연결 -->
<img :src="'/path/to/images/' + fileName" />

<!-- CSS 클래스 바인딩 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
<!-- 삼항 연산자를 사용할 수 있다. 콤마 뒤는 디폴트값이다. -->

<!-- CSS 스타일 바인딩 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>
```

css 클래스 바인딩을 할 때 주의할 점은 오브젝트 형태로 값을 전달해야 한다는 점이다. 특히 키와 값의 형태로 작성을 해줘야 하는데 키에는 클래스명을, 값에는 boolean이나 조건식을 통해 조건부로 클래스를 적용시킬 수 있다. 

## 조건부 렌더링

```html
<template>
  <div>
    <input type="text" v-model="age" />
    <h2>당신의 나이는 {{age}}입니다.</h2>
    <p v-if="age>19">당신은 어른입니다.</p>
    <p v-else-if="age>0 && age<19">당신은 어른이 아닙니다.</p>
    <p v-else>당신은 어린이입니다.</p>
  </div>
</template>
```

```javascript
<script>
export default {
  name: 'App',
  data:()=>{
    return {
      age:10,
      }
    }
  }
</script>
```

### V-if

표현식 값의 참-거짓에 따라 엘리먼트를 조건부로 렌더링한다.

### V-else-if

V-if와 마찬가지로 표현식을 적용시킬 수 있지만 형제 엘리먼트에 v-if 또는 v-else-if가 있어야 한다.

### V-else

표현식을 요구하지 않지만, 형제 엘리먼트에 v-if 또는 v-else-if가 있어야 한다. 

___

주의해야할 점이 있다면 항상 같은 부모 엘리먼트 아래, 즉 형제 엘리먼트의 관계가 형성되어야만 V-if, V-else-if, V-else를 사용할 수 있다는 것이다. 또한 조건 디렉티브 사이에 다른 엘리먼트가 들어가게 되면 오류가 발생한다.

### V-show

```html
<template>
  <h1 v-show="display">보인다!</h1>
</template>
```

```javascript
<script>
export default {
  name: 'App',
  data:()=>{
    return {
      display:false,
      }
    }
  }
</script>
```

표현식 값의 참-거짓을 기반으로 엘리먼트의 `display` CSS 속성을 전환한다.

## :bulb:그러면 V-if 랑 V-show 중에 뭘 써야할까?

`v-if`는 "실제(real)" 조건부 렌더링이다. 조건에 따라 조건부 블록 내부의 이벤트 리스너 및 자식 컴포넌트들이 제거되고 다시 생성되기 때문이다.

`v-if`는 초기 렌더링 시, 조건이 거짓(false)이면 아무 작업도 하지 않는다. 조건부 블록은 조건이 처음으로 참(true)이 될 때까지 렌더링되지 않는다.

이에 비해 `v-show`는 훨씬 간단하다. 엘리먼트는 CSS 기반 전환으로 초기 조건과 관계 없이 항상 렌더링된다. v-show는 엘리먼트를 DOM에 우선 렌더링하고 조건에 따라 CSS display 속성을 전환한다.

일반적으로 `v-if`는 전환 비용이 높은 반면, `v-show`는 초기 렌더링 비용이 높다. 그러므로 무언가를 자주 전환해야 한다면 `v-show`를 사용하는 게 좋고, 런타임 시 조건이 변경되지 않는다면 `v-if`를 사용하는 게 더 낫다.

## 반복문

```html
<template>
  <h1 v-for="animal in animals" :key="animal">{{animal}}</h1>
</template>
```

```javascript
<script>
export default {
  name: 'App',
  data:()=>{
    return {
      animals: ["dog", "cat", "lion", "giraffe"]
      }
    },
  }
</script>
```

### v-for

원본 데이터를 기준으로 엘리먼트 또는 템플릿 블록을 여러번 렌더링 한다. 신경 써야할 부분은 key 속성을 추가해주어야 한다는 점이다. 

특수한 속성인 `key`는 주로 Vue의 가상 DOM 알고리즘이 노드의 ID를 식별하기 위해 사용된다. 이를 통해 Vue는 어느 시점에 기존 노드를 가져오고 재사용할 수 있는지, 또 재정렬과 재생성이 필요한 때가 언제인지 파악한다. 정리하자면 성능상의 이유로 key를 설정하는 것이 좋다는 것이다.

```html
<div v-for="(item, index) in items"></div>
<div v-for="(value, key) in objects"></div>
<div v-for="(value, index) in objects"></div>
```

그리고 위와 같이 index, key 등 다양한 값들을 함께 불러올 수 있다.

# :books:참고자료

[디렉티브(Directives) | Vue.js](https://v3.ko.vuejs.org/api/directives.html#v-text)

[조건부 렌더링 | Vue.js](https://v3.ko.vuejs.org/guide/conditional.html#v-if)

[Vue.js의 기본 디렉티브 정리](https://uxgjs.tistory.com/112)
