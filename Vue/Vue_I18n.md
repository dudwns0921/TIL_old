# Vue

## I18n

Vue I18n은 Vue.js의 국제화 플러그인이다. 현지화 기능을 Vue.js 애플리케이션에 쉽게 통합할 수 있다.

### 시작하기

#### 1. 변역된 문구들을 준비한다.

이 때 문구는 계층적 객체 구조를 가지며 각 `locale`를 최상위 속성으로 갖는다. 

```js
const messages = {
  en: {
    message: {
      hello: 'hello world'
    }
  },
  ja: {
    message: {
      hello: 'こんにちは、世界'
    }
  }
}
```

#### 2. I18n 인스턴스를 만든다. 아래 예시는 가장 기본적인 형태이다.

```js
const i18n = VueI18n.createI18n({
  locale: 'ja', // locale 설정
  fallbackLocale: 'en', // default locale 설정
  messages,
})
```

#### 3. I18n 옵션과 함께 Vue 인스턴스를 생성한다.

```js
new Vue({ i18n }).$mount('#app')
```

만약 module 시스템을 사용 중이라면 아래와 같이 할 수 있다.

```js
import Vue from 'vue'
import VueI18n from 'vue-i18n'

Vue.use(VueI18n)
```

`<template>`에서는 아래와 같이 사용이 가능하다.

```vue
<div id="app">
	<p>{{ $t("message.hello") }}</p>
</div>
```

추가적으로 I18n 옵션과 함께 Vue 인스턴스를 생성한 후 각 컴포넌트에서 `this.$i18n`으로 Vuel18n 인스턴스에 접근할 수 있다. 싱글 파일 컴포넌트에서 `<template>`이 아닌 `<script>`에서 자바스크립트 코드들과 함께 I18n을 사용할 때 필요하다.

## 회사 코드 살펴보기

```vue
<div v-active class="btn-today" @click="onBtnToDay">
    {{ this.$i18n.t("Today") }}
</div>
```

회사 소스 코드를 살펴보니 템플릿 내에서도 this.$i18n으로 접근하고 있었다. 이렇게 해도 문제는 없지만 템플릿 내에서는 다음과 같이 쓰는 게 훨씬 편하다고 생각한다.

```vue
<div v-active class="btn-today" @click="onBtnToDay">
    {{ $t("Today") }}
</div>
```

# :books:참고자료

https://kazupon.github.io/vue-i18n/introduction.html

https://vue-i18n.intlify.dev/guide/introduction.html


