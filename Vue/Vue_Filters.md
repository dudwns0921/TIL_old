# Filters

뷰의 필터는 화면에 표시되는 텍스트의 형식을 쉽게 변환해주는 기능이다. 가장 간단하게는 단어의 대문자화부터 다국어, 국제 통화 표시 등 다양하게 사용할 수 있다.

## 필터 사용 방법

필터틀 사용하는 방법은 아래와 같다.

```javascript
<div id="app">
  {{ message | capitalize }}
</div>
```

```javascript
new Vue({
  el: '#app',
  data: {
    message: 'hello'
  },
  filters: {
    capitalize: function(value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```

위의 코드를 실행하면 Hello 텍스트가 화면에 출력된다.

## 필터 등록 패턴

위와 같이 `filters` 속성을 이용하여 각 컴포넌트에 필터를 등록하는 방법도 있지만, 실제로는 대부분 filters.js 파일을 별도로 분리하여 사용합니다. 아래와 같이 말이죠.

```javascript
// filters.js
export function capitalize() {
  // ..
}

export function translate() {
  // ..
}
```

```javascript
// main.js
import Vue from 'vue';
import * as filters from 'filters.js';

Object.keys(filters).forEach(function(key) {
  Vue.filter(key, filters[key]);
});

new Vue({
  // ..
})
```

### 필터 체이닝

```javascript
{{ message | filterA | filterB }}
```

이렇게 여러 개의 필터가 적용되는 경우, 작성한 순서대로 필터가 적용된다. FilterA에서 message의 값을 받고, FilterB에서는 FilterA의 결과물을 받는다.

### 인수 사용 가능

```javascript
{{ message | filterA('arg1', arg2) }}
```

filter는 자바스크립트 함수이기 때문에 인수를 가질 수 있다. 이 때 filter의 첫 번째 인수의 값은 무조건 해당 필터를 적용한 변수가 된다. 이 경우에는 message가 첫 번째 인수로 전달된다. 그 다음에 순서대로 2번째, 3번째 인수가 전달된다.

## Vue 3.x 변경점

3.x 버전에서는 filter는 삭제되었고 더 이상 지원되지 않는다. 대신에 method 호출이나 computed 속성으로 대체하도록 권장하고 있다.

[Filters | Vue.js](https://v3.ko.vuejs.org/guide/migration/filters.html#%E1%84%80%E1%85%A2%E1%84%8B%E1%85%AD)

### 전역 Filters

애플리케이션에 filters를 전역적으로 등록하고 사용하는 경우, 각각의 컴포넌트에서 computed 속성이나 methods로 대체하는 것은 불편할 수 있다.

대신, [globalProperties](https://v3.ko.vuejs.org/api/application-config.html#globalproperties)를 통해 모든 컴포넌트에 전역 filters를 사용할 수 있다.

```javascript
// main.js
const app = createApp(App)

app.config.globalProperties.$filters = {
  currencyUSD(value) {
    return '$' + value
  }
}
```

그런 다음에 다음과 같이 `$filters`객체를 사용하여 모든 템플릿을 수정할 수 있다.

```html
<template>
  <h1>Bank Account Balance</h1>
  <p>{{ $filters.currencyUSD(accountBalance) }}</p>
</template>
```

이 접근방식에서는 computed 속성이 아닌 methods만 사용할 수 있습니다. 후자는 개별 컴포넌트의 컨텍스트에서 정의된 경우에만 의미가 있다.
