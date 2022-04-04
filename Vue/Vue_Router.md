# Vue Router

## 설치하기

> npm install vue-router --save

## 라우터 생성하기

아래 코드들은 vue cli를 사용해 만든 프로젝트의 일부임을 참고바란다.

먼저 라우터를 독립적으로 관리할 수 있도록 router.js 파일을 따로 생성해준다.

```javascript
// router.js

import HomePg from '../pages/HomePg.vue';
import SearchPg from '../pages/SearchPg.vue';
import { createWebHashHistory, createRouter } from 'vue-router';

const routes = [
  { path: '/', component: HomePg },
  { path: '/search', component: SearchPg }
];
const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

```javascript
// main.js

import { createApp } from 'vue';
import App from './App.vue';
import router from './router/router.js';

const myApp = createApp(App);
myApp.use(router);
myApp.mount('#app');


```

라우터를 사용하기 위해서는 총 4단계가 필요하다.

1. 렌더링할 컴포넌트를 임포트한다.

2. 라우트(경로)들을 작성한다.

각각의 경로는 객체로 이루어져있으며, path와 component 속성을 갖는다. path에는 url를, component에는 해당 주소에서 렌더링할 컴포넌트를 적어주면 된다.

3. createRouter() 메서드를 통해 라우터를 생성한다.

여러 옵션을 정의할 수 있지만 공식 문서에서 가장 기본적인 옵션인 history와 routes를 정의하고 있다. 

routes, 는 routes: routes을 줄인 것으로 객체의 속성과 값이 같을 때 축약해서 작성할 수 있다. 

4. main.js에서 router을 임포트하고 use() 메서드로 라우터를 사용한다.

이제 라우터는 정상적으로 만들어졌다. 해당 라우터를 사용하기 위해서 App.vue 파일로 가보자.

## 렌더링

```html
<!-- app.vue -->

<template>
  <div class="app">
    <router-view></router-view>
  </div>
</template>
```

use를 통해 라우터를 사용한 컴포넌트에서 router-view 태그를 선언하면 URL에 맞게 컴포넌트들이 맵핑된다.

## 라우터 링크

사용자에게 라우팅 된 경로로 이동하게끔 앵커태그를 생성하는 속성이다. `router-link`속성을 통해 앵커 태그 기능을 수행하게한다.

```html
<router-link to="/">Home</router-link>
```

## 스타일링

`scoped`속성을 통해 에서 해당 컴포넌트에서만 독립하게 사용하는 스타일을 입힐 수 있는데, 라우터 링크에서 사용되는 앵커태그의 기본 값은 `router-link-exact-active`이며, active-class속성을 통해 따로 정의할 수도 있다.

위에 작성한 코드를 스타일링한다고 가정해보자.

```html
<style>
.router-link-exact-active{
    text-decoration: none;
}    
</style>
```



# :books:참고자료

[Getting Started | Vue Router](https://router.vuejs.org/guide/)

[Vue 라우터 개념 및 사용방법](https://jinyisland.kr/post/vue-router/)
