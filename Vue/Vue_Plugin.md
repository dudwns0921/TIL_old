# Vue

## Plugin

플러그인은 애플리케이션에서 자주 사용될만한 속성, 함수, 라이브러리 등의 사용성을 높여주는 기능이다.

### 사용하기

플러그인은 전역 메서드인 `Vue.use()`를 호출하는 것으로 사용할 수 있다. 이 작업은 사용자의 `Vue` 생성자를 호출해 애플리케이션을 시작하기 전에 끝나야 한다.

```js
import ChartPlugin from './ChartPlugin.js';

Vue.use(ChartPlugin);

new Vue({
  // ...
});
 
```

 `vue-router`와 같이 `Vue.js` 공식 플러그인에 의해 제공되는 몇몇 플러그인의 경우, `Vue`를 전역 변수로 사용할 수 있는지 여부에 따라 자동으로  `Vue.use` 메서드를 호출한다. 하지만 `CommonJS`와 같은 `module` 환경에서는 항상 명시적으로 `Vue.use()`를 호출해야 한다.

```js
import VueRouter from 'vue-router'
import Vue from 'vue'

Vue.use(VueRouter)
```

## Plugin 구현하기

```js
// ChartPlugin.js
import Chart from 'chartjs';

export default {
  install(Vue, options) {
    Vue.prototype.ChartJS = Chart;
  }
}
```

위 코드는 `chartjs`라는 외부 라이브러리를 `npm` 방식으로 프로젝트에 설치한 후 해당 라이브러리를 플러그인으로 사용하는 코드이다. 차트 라이브러리를 불러와 `Chart`라는 변수에 담고, 뷰 플러그인을 설치(install)할 때 뷰의 프로토타입 속성으로 해당 변수를 연결하는 코드이다. 따라서 아래와 같이 `Vue.use()`메서드를 통해 플러그인을 설치하면 컴포넌트에서 매번 차트 라이브러리를 불러오지 않고도 사용할 수 있다.

## :bulb:Tip - 플러그인 변수명

플러그인 변수명은 `$_`가 좋다. 뷰 라이브러리 내부적으로 사용하는 private 변수는 `_`를 사용하고 있고, 사용자에게 노출시키는 인스턴스 관련 속성은 `$`를 사용하고 있기 때문이다. 이 내용은 뷰 공식 문서의 스타일 가이드에서 찾아볼 수 있다.

# :books:참고자료

https://v2.vuejs.org/v2/guide/plugins.html

https://joshua1988.github.io/vue-camp/reuse/plugins.html#%E1%84%91%E1%85%B3%E1%86%AF%E1%84%85%E1%85%A5%E1%84%80%E1%85%B3%E1%84%8B%E1%85%B5%E1%86%AB-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC-%E1%84%87%E1%85%A1%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8