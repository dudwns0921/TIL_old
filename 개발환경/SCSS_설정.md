# Scss 설정

Vue cli 3 이상부터는 프로젝트를 생성할 때 scss 관련 기능을 쉽게 추가할 수 있다. 다만 프로젝트 중간에 설치할 경우에는 패키지를 따로 설치해야 한다.

## scss 패키지 설치

해당 package에 node-sass와 sass-loader를 설치해준다.

```jsx
npm install --save-dev node-sass sass-loader
```

## :bulb:Tip

scss와 Sass의 차이를 모를 경우, scss를 컴파일하는데 왜 sass-loader을 설치하지? 라는 의문을 들 수도 있다. 

Sass(Syntactically Awesome Style Sheets)의 3버전에서 새롭게 등장한 SCSS는 CSS 구문과 완전히 호환되도록 새로운 구문을 도입해 만든 Sass의 모든 기능을 지원하는 CSS의 상위집합(Superset) 이다. 결국 SCSS 안에 Sass가 포함된다고 생각하면 된다.

### 사용법

설치를 완료했다면 끝입니다! 이제 바로 컴포넌트 내에서 scss를 사용할 수 있다. 간단한 설치 만으로 사용이 가능한 이유는 vue-loader에서 기본으로 설정되어 있는 webpack 설정 때문이다. 패키지 설치 후 컴포넌트 내에서 lang 속성을 지정해주면 자동으로 Loader를 사용하여 바로 사용할 수 있습니다.

### 전역 스타일 및 변수 설정

```html
<style lang="scss">
  @import "@/asstes/scss/파일명";
</style>

<!-- 모든 파일마다 적용시켜줘야 함-->
```

변수를 담아둔 scss파일을 매번 컴포넌트에서 불러와서 사용하는 것은 매우 불편한 방법이고 효율적이지 않다. 따라서 자주 사용하는 변수나 reset스타일, mixin같은 경우 전역 스타일을 설정하여 사용이 가능하다.

설정 방법은 프로젝트 최상단에 vue.config.js 파일을 생성한 후 webpack 설정에 css 관련 설정을 추가해주면 된다. sass-loader 버전에 따라서 설정법이 조금씩 다른데 전체적인 구조는 같지만 8버전에서는 prependData로 선언해야 하고 그 이상의 버전에서는 아래 처럼 additionalData를 선언하면 전역으로 scss이 적용된다.

```javascript
module.exports = {
  css : {
    loaderOptions : {
      sass : {
        additionalData: `
          @import "@/assets/scss/_variables.scss";
        `
      }
    }
  }
}
```

# :books:참고자료

[Sass(SCSS) 완전 정복! | HEROPY](https://heropy.blog/2018/01/31/sass/)

https://velog.io/@function_dh/Vue-scss-%EC%B6%94%EA%B0%80-%EB%B0%8F-%EC%A0%84%EC%97%AD-%EC%8A%A4%ED%83%80%EC%9D%BC-%EC%84%A4%EC%A0%95
