# 웹팩의 4가지 주요속성

웹팩의 빌드(파일 변환) 과정을 이해하기 위해서는 아래 4가지 주요 속성에 대해서 알고 있어야 한다.

- entry
- output
- loader
- plugin 

## Entry

`entry` 속성은 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점이자 자바스크립트 파일 경로이다.

```javascript
// webpack.config.js
module.exports = {
  entry: './src/index.js'
}
```

위 코드는 웹팩을 실행했을 때 `src` 폴더 밑의 `index.js` 을 대상으로 웹팩이 빌드를 수행하는 코드이다.

### Entry 파일에는 어떤 내용이 들어가야 하나?

`entry` 속성에 지정된 파일에는 웹 애플리케이션의 전반적인 구조와 내용이 담겨져 있어야 한다. 웹팩이 해당 파일을 가지고 웹 애플리케이션에서 사용되는 모듈들의 연관 관계를 이해하고 분석하기 때문에 애플리케이션을 동작시킬 수 있는 내용들이 담겨져 있어야 한다.

예를 들어, 블로그 서비스를 웹팩으로 빌드한다고 했을 때 코드의 모양은 아래와 같을 수 있다.

```javascript
// index.js
import LoginView from './LoginView.js';
import HomeView from './HomeView.js';
import PostView from './PostView.js';

function initApp() {
  LoginView.init();
  HomeView.init();
  PostView.init();
}

initApp();
```

위 코드는 해당 서비스가 SPA이라고 가정하고 작성한 코드입니다. 사용자의 **로그인 화면**, 로그인 후 진입하는 **메인 화면**, 그리고 **게시글을 작성하는 화면** 등 웹 서비스에 필요한 화면들이 모두 `index.js` 파일에서 불려져 사용되고 있기 때문에 웹팩을 실행하면 해당 파일들의 내용까지 해석하여 파일을 빌드해줄 것이다.

### Entry 유형

앞에서 살펴본 것처럼 엔트리 포인트는 1개가 될 수도 있지만 아래와 같이 여러 개가 될 수도 있다.

```javascript
entry: {
  login: './src/LoginView.js',
  main: './src/MainView.js'
}
```

위와 같이 엔트리 포인트를 분리하는 경우는 싱글 페이지 애플리케이션이 아닌 특정 페이지로 진입했을 때 서버에서 해당 정보를 내려주는 형태의 멀티 페이지 애플리케이션에 적합합니다. 특히 엔트리 포인트 사이에 코드/모듈을 많이 재사용하는 멀티 페이지 애플리케이션의 경우에 많은 이점이 있다.

## Output

`output` 속성은 웹팩을 돌리고 난 결과물의 파일 경로를 의미한다.

```javascript
// webpack.config.js
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
```

앞에서 배운 `entry` 속성과는 다르게 객체 형태로 옵션들을 추가해야 한다.

### Output 속성 옵션 형태

최소한 `filename`은 지정해줘야 하며 일반적으로 아래와 같이 `path` 속성을 함께 정의합니다.

```javascript
// webpack.config.js
var path = require('path');

module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
// __dirname은 현재 실행 중인 폴더 경로
  }
}
```

여기서 `filename` 속성은 웹팩으로 빌드한 파일의 이름을 의미하고, `path` 속성은 해당 파일의 경로를 의미한다.

그리고 `path` 속성에서 사용된 `path.resolve()` 코드는 인자로 넘어온 경로들을 조합하여 유효한 파일 경로를 만들어주는 Node.js API입니다. 이 API가 하는 역할을 좀 더 이해하기 쉽게 표현하면 아래와 같습니다.

```
output: './dist/bundle.js'
```

### Output 파일 이름 옵션

앞에서 살펴본 `filename` 속성에 여러 가지 옵션을 넣을 수 있다.

1. 결과 파일 이름에 `entry` 속성을 포함하는 옵션

```javascript
module.exports = {
  output: {
    filename: '[name].bundle.js'
  }
};
```

2. 결과 파일 이름에 웹팩 내부적으로 사용하는 모듈 ID를 포함하는 옵션

```javascript
module.exports = {
  output: {
    filename: '[id].bundle.js'
  }
};
```

3. 매 빌드시 마다 고유 해시 값을 붙이는 옵션

```javascript
module.exports = {
  output: {
    filename: '[name].[hash].bundle.js'
  }
};
```

4. 웹팩의 각 모듈 내용을 기준으로 생생된 해시 값을 붙이는 옵션

```javascript
module.exports = {
  output: {
    filename: '[chunkhash].bundle.js'
  }
};
```

이렇게 생성된 결과 파일의 이름에는 각각 엔트리 이름, 모듈 아이디, 해시 값 등이 포함됩니다. 해당 옵션을 쓰는 이유에 대해서는 [웹팩 캐싱 전략](https://joshua1988.github.io/web-development/webpack/caching-strategy/)을 한 번 참고해보자. 

## Loader

로더(Loader)는 웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images, 폰트 등)들을 변환할 수 있도록 도와주는 속성이다.

```javascript
// webpack.config.js
module.exports = {
  module: {
    rules: []
  }
}
```

엔트리나 아웃풋 속성과는 다르게 `module`라는 이름을 사용한다.

### Loader가 필요한 이유

웹팩으로 애플리케이션을 빌드할 때 만약 아래와 같은 코드가 있다고 해보겠습니다.

```javascript
// app.js
import './common.css';

console.log('css loaded');
```

```javascript
/* common.css */
p {
  color: blue;
}
```

```javascript
// webpack.config.js
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  }
}
```

위 파일을 웹팩으로 빌드하게 되면 아래와 같은 에러가 발생한다.

![CSS 로딩 에러](https://joshua1988.github.io/webpack-guide/assets/img/css-loading-error.a03a18eb.png)

위 에러 메시지의 의미는 `app.js` 파일에서 임포트한 `common.css` 파일을 해석하기 위해 적절한 로더를 추가해달라는 것이다.

### CSS Loader 적용하기

이 때 해당 폴더에 아래의 NPM 명령어로 CSS 로더를 설치하고 웹팩 설정 파일 설정을 바꿔주면 에러를 해결할 수 있습니다.

```javascript
npm i css-loader -D
```

```javascript
// webpack.config.js
module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['css-loader']
      }
    ]
  }
}
```

위의 `module` 쪽 코드를 보면 `rules` 배열에 객체 한 쌍을 추가했습니다. 그리고 그 객체에는 2개의 속성이 들어가 있는데 각각 아래와 같은 역할을 한다.

- `test` : 로더를 적용할 파일 유형 (일반적으로 정규 표현식 사용)
- `use` : 해당 파일에 적용할 로더의 이름

### 자주 사용되는 로더 종류

앞에서 살펴본 CSS 로더 이외에도 실제 서비스를 만들 때 자주 사용되는 로더의 종류는 다음과 같습니다.

- [Babel Loader](https://webpack.js.org/loaders/babel-loader/#root)
- [Sass Loader](https://webpack.js.org/loaders/sass-loader/#root)
- [File Loader](https://webpack.js.org/loaders/file-loader/#root)
- [Vue Loader](https://github.com/vuejs/vue-loader)
- [TS Loader](https://webpack.js.org/guides/typescript/#loader)

로더를 여러 개 사용하는 경우에는 아래와 같이 `rules` 배열에 로더 옵션을 추가해주면 된다.

```javascript
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' },
      // ...
    ]
  }
}
```

### 로더 적용 순서

특정 파일에 대해 여러 개의 로더를 사용하는 경우 로더가 적용되는 순서에 주의해야 합니다. 로더는 기본적으로 **오른쪽에서 왼쪽 순으로 적용**된다.

CSS의 확장 문법인 SCSS 파일에 로더를 적용하는 예시를 보자.

```javascript
module: {
  rules: [
    {
      test: /\.scss$/,
      use: ['css-loader', 'sass-loader']
    }
  ]
}
```

위 코드는 scss 파일에 대해 먼저 Sass 로더로 전처리(scss 파일을 css 파일로 변환)를 한 다음 웹팩에서 CSS 파일을 인식할 수 있게 CSS 로더를 적용하는 코드이다.

만약 웹팩으로 빌드한 자원으로 실행했을 때 해당 CSS 파일이 웹 애플리케이션에 인라인 스타일 태그로 추가되는 것을 원한다면 아래와 같이 [style 로더](https://webpack.js.org/loaders/style-loader/#root)도 추가할 수 있다.

```javascript
{
  test: /\.scss$/,
  use: ['style-loader', 'css-loader', 'sass-loader']
}
```

그리고, 위와 같이 배열로 입력하는 대신 아래와 같이 옵션을 포함한 형태로도 입력할 수 있다.

```javascript
module: {
  rules: [
    {
      test: /\.css$/,
      use: [
        { loader: 'style-loader' },
        {
          loader: 'css-loader',
          options: { modules: true }
        },
        { loader: 'sass-loader' }
      ]
    }
  ]
}
```

## Plugin

플러그인(plugin)은 웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성이다. 로더랑 비교하면 로더는 파일을 해석하고 변환하는 과정에 관여하는 반면, 플러그인은 해당 결과물의 형태를 바꾸는 역할을 한다고 보면 된다.

플러그인은 아래와 같이 선언한다.

```javascript
// webpack.config.js
module.exports = {
  plugins: []
}
```

플러그인의 배열에는 생성자 함수로 생성한 객체 인스턴스만 추가될 수 있다.

```javascript
// webpack.config.js
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin(),
    new webpack.ProgressPlugin()
  ]
}
```

위의 두 플러그인은 각각 아래와 같은 역할을 다.

- [HtmlWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/) : 웹팩으로 빌드한 결과물로 HTML 파일을 생성해주는 플러그인
- [ProgressPlugin](https://webpack.js.org/plugins/progress-plugin/#root) : 웹팩의 빌드 진행율을 표시해주는 플러그인

### 자주 사용하는 플러그인

- [split-chunks-plugin](https://webpack.js.org/plugins/split-chunks-plugin/)
- [clean-webpack-plugin](https://www.npmjs.com/package/clean-webpack-plugin)
- [image-webpack-loader](https://github.com/tcoopman/image-webpack-loader)
- [webpack-bundle-analyzer-plugin](https://github.com/webpack-contrib/webpack-bundle-analyzer)

## Concepts Review

여태까지 살펴본 웹팩 4가지 주요 속성을 도식으로 나타내보면 다음과 같다.

![웹팩 도식](https://joshua1988.github.io/webpack-guide/assets/img/diagram.519da03f.png)

1. **Entry** 속성은 웹팩을 실행할 대상 파일. 진입점
2. **Output** 속성은 웹팩의 결과물에 대한 정보를 입력하는 속성. 일반적으로 `filename`과 `path`를 정의
3. **Loader** 속성은 CSS, 이미지와 같은 비 자바스크립트 파일을 웹팩이 인식할 수 있게 추가하는 속성. 로더는 오른쪽에서 왼쪽 순으로 적용
4. **Plugin** 속성은 웹팩으로 변환한 파일에 추가적인 기능을 더하고 싶을 때 사용하는 속성. 웹팩 변환 과정 전반에 대한 제어권을 갖고 있음

# :books:참고자료

[Introduction | 웹팩 핸드북](https://joshua1988.github.io/webpack-guide/guide.html)
