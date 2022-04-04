# Computed & Watch

## Computed 속성

컴퓨티드(computed) 속성은 템플릿의 데이터 표현을 더 직관적이고 간결하게 도와주는 속성이다. 좀 더 간단히 말하자면 템플릿의 가독성을 위해 사용하는 속성이다.

밑의 예시를 통해 Computed 속성에 따라 가독성이 얼마나 차이나는지 확인해보자.

```html
<template>
  <div class="body">
    <h1>{{ author.books.length > 0 ? '출판한 책이 있음' : '출판한 책이 없음' }}</h1>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      author: {
        name: '존 도우',
        books: ['Vue 2 - Advanced Guide', 'Vue 3 - Basic Guide', 'Vue 4 - The Mystery'],
      },
    };
  },
};
</script>
```

전체적인 흐름을 보자.

1. 작가가 출판한 책이 있는지 없는지에 따라 다른 텍스트를 보여주고자 한다.

2. 판단은 author 객체의 books 속성의 길이를 통해 이루어진다.

이 시점에서 템플릿은 더 이상 단순하고 선언적이지 않다. 물론 가독성을 해치지 않는 간단한 연산이라면 큰 문제는 없겠지만 연산이 길어질수록 vue의 핵심이라고 할 수 있는 '선언적' 이라는 개념에서 멀어지게 되는 것이다.

그렇다면 Computed 속성을 사용해서 이 문제를 해결해보자.

```html
<template>
  <div class="body">
    <h1>{{ publishedBooks }}</h1>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      author: {
        name: '존 도우',
        books: ['Vue 2 - Advanced Guide', 'Vue 3 - Basic Guide', 'Vue 4 - The Mystery'],
      },
    };
  },
  computed: {
    publishedBooks() {
      return this.author.books.length > 0 ? '출판한 책이 있다' : '출판한 책이 없다';
    },
  },
};
</script>
```

위의 코드를 통해 템플릿의 가독성이 월등히 좋아진 것을 확인할 수 있다. Computed 속성은 템플릿의 가독성을 높여준다는 장점뿐만 아니라 Computed 속성의 대상으로 정한 data 속성이 변했을 때 이를 감지하고 자동으로 다시 연산해주는 장점이 있다.

위의 코드에 적용시켜서 설명하자면,

1. publishedBooks의 대상은 author의 books 속성이다.

2. books 배열의 길이가 변하게 되면 publishedBooks의 속성도 변경된다. 

## Watch 속성

watch 속성은 특정 데이터의 변화를 감지하여 자동으로 특정 로직을 수행해주는 속성이다.

```html
<template>
  <div class="body">
    <input type="text" v-model="message" />
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      message: 'Hello',
    };
  },
  watch: {
    message: function (value) {
      console.log(value);
    },
  },
};
</script>
```

사용방법은 다음과 같다. watch 속성에 감지할 데이터의 명으로 속성을 만든 후 함수를 정의해주면 된다. 함수의 첫 번째 파라미터는 감지하는 데이터가 된다. 

## Computed와 Watch, 언제 써야할까?

많은 자료들을 통해 살펴봤지만, 결국 템플릿에 각각의 속성을 사용할지 말지에 따라 그 용도가 구분된다고 생각한다.

Computed의 가장 기본적인 목적은 템플릿의 가독성을 향상시키는 것이다. 그렇기 때문에 당연하게도 템플릿에 사용해야만 한다. 

wathc는 특정 데이터의 변화를 감지하여 로직을 수행해주는 것이 목적이다. 로직을 수행하는데 그 목적이 있기 때문에 watch 속성은 템플릿에 사용할 이유가 없다.

어떻게 보면 너무 뻔하고 단순한 차이같지만 여러 자료들을 보면서 직관적으로 받아들이게 된 둘의 차이점이기도 하다.

# :books:참고자료

[Computed | Cracking Vue.js](https://joshua1988.github.io/vue-camp/syntax/computed.html)

[Computed 속성과 Watch | Vue.js](https://v3.ko.vuejs.org/guide/computed.html#computed-%E1%84%89%E1%85%A9%E1%86%A8%E1%84%89%E1%85%A5%E1%86%BC)

https://medium.com/@hozacho/%EB%A7%A8%EB%95%85%EC%97%90vuejs-computed-vs-watch-%EC%96%B8%EC%A0%9C%EC%8D%A8%EC%95%BC%ED%95%A0%EA%B9%8C-d25316c4ef42
