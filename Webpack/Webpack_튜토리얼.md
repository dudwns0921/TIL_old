# 웹팩 튜토리얼

### 웹팩 적용 전

1. 빈 폴더에서 아래 명령어로 `package.json` 파일을 생성

```bash
npm init -y
```

2. 아래 명령어로 해당 폴더에 웹팩 관련 라이브러리와 *lodash* 라이브러리 설치

```bash
npm i webpack webpack-cli -D
npm i lodash
```

3. 폴더에 `index.html` 파일을 생성하고 아래 내용 추가

```html
<html>
  <head>
    <title>Webpack Demo</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="src/index.js"></script>
  </body>
</html>
```

4. 프로젝트 루트 레벨에 `src` 폴더를 생성하고 그 안에 `index.js` 파일 생성.

```javascript
function component() {
  var element = document.createElement('div');

  /* lodash is required for the next line to work */
  element.innerHTML = _.join(['Hello','webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

### 웹팩 적용

1. 웹팩 빌드 및 빌드 결과물로 실행하기 위해 각 파일에 아래 내용 반영

```javascript
// index.js
import _ from 'lodash';

function component() {
  var element = document.createElement('div');

  /* lodash is required for the next line to work */
  element.innerHTML = _.join(['Hello','webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

```html
<!-- index.html -->
<html>
  <head>
    <title>Webpack Demo</title>
    <!-- <script src="https://unpkg.com/lodash@4.16.6"></script> -->
  </head>
  <body>
    <!-- <script src="src/index.js"></script> -->
    <script src="dist/main.js"></script>
  </body>
</html>
```

2. 웹팩 빌드 명령어를 실행하기 위해 `package.json` 파일에 아래 내용 추가

```json
"scripts": {
  "build": "webpack --mode=none"
}
```

3. `npm run build` 명령어 실행 후 `index.html` 파일을 [라이브서버](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)로 실행

package.json 파일에다가 webpack 관련 설정들을 계속 하기보다는 webpack 설정 파일을 따로 만들어 관리해주는 것이 좋다.

### webpack.config.js 파일 생성

1. 프로젝트 폴더 루트 레벨에 `webpack.config.js` 파일 생성 후 아래 내용 추가

```javascript
// webpack.config.js
// `webpack` command will pick up this config setup by default
var path = require('path');

module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

9. `package.json` 파일을 아래와 같이 수정

```json
"scripts": {
  "build": "webpack"
}
```

10. 다시 `npm run build` 명령어를 실행하여 빌드가 잘 되는지 확인

웹팩 적용 전후 비교

### 적용 전

![](file://C:\Users\220307\OneDrive - Obigo Inc\바탕 화면\TIL\Webpack\md-images\2022-04-11-15-13-48-image.png?msec=1649657628182?msec=1649661026000)

### 적용 후

![](file://C:\Users\220307\OneDrive - Obigo Inc\바탕 화면\TIL\Webpack\md-images\2022-04-11-15-12-07-image.png?msec=1649657527697?msec=1649661026000)

네트워크 패널 상으로 두 실습의 차이를 살펴보자. 가장 큰 차이점은 HTTP 요청의 개수가 줄었다는 점이다. HTTP 요청 숫자를 줄이면 웹 애플리케이션의 성능을 높여줄 뿐만 아니라 사용자가 사이트를 조작하는 시간을 앞당겨 줄 수 있게 된다.
