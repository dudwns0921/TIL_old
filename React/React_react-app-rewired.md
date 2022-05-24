# React

## CRA 빌드 설정 커스터마이징

> Create React App is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration.
>
> CRA는 SPA를 만들기 위해 공식적으로 지원되는 방법입니다. CRA는 어떠한 설정 없이도 최신의 빌드 환경을 제공합니다.

아마 많은 사람들이 처음 리액트 애플리케이션을 만들 때 CRA를 사용했을 거라고 생각한다. 위에서 말한 것처럼 최신의 빌드 환경을 제공해주기 때문에 우리가 해야할 것은 UI에 대한 코드를 작성하는 것뿐이다. 하지만 빌드 환경에 대한 커스터마이징이 필요하게 되면 어떻게 해야할까?

### react-scripts eject

첫 번째로 해볼 수 있는 방법은 eject 명령어를 사용하는 것이다. CRA는 설정파일을 숨기기 때문에 eject 명령어를 통해서 설정파일들을 추출해야 설정을 변경할 수 있다. eject 명령어는 영구적이기 때문에 실행하면 설정 파일들을 다시 숨길 수는 없다.

자, 이제 커스터마이징을 하면 된다. 하지만 이렇게 되면 애초에 CRA를 쓴 목적이 퇴색된다고 생각한다. 복잡한 설정을 피하기 위해 CRA를 썼는데 다시 그 복잡한 설정을 하나하나 확인해가며 커스터마이징을 해야한다니... 여기서 이번에 알아볼 react-app-rewired가 등장한다. 기존의 설정에 추가된 설정을 덮어쓰는 일종의 꼼수(?)라고 생각하면 된다.

### react-app-rewired

#### 설치

##### For create-react-app 2.x with Webpack 4:

```bash
npm install react-app-rewired --save-dev
```

##### For create-react-app 1.x or react-scripts-ts with Webpack 3:

```bash
npm install react-app-rewired@1.6.2 --save-dev
```

#### 루트 디렉토리에 설정 파일 만들기

```js
// config-overrides.js

module.exports = function override(config, env) {
  //do stuff with the webpack config...
  return config;
}
```

#### package.json에서 명령어 변경

```json
// package.json

  "scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test",
+   "test": "react-app-rewired test",
    "eject": "react-scripts eject"
}
```

`-`에서 `+`의 상태로 바꿔주면 된다.

### 실제 사용해보기

> By default, the `config-overrides.js` file exports a single function to use when customising the webpack configuration for compiling your react app in development or production mode. It is possible to instead export an object from this file that contains up to three fields, each of which is a function. This alternative form allows you to also customise the configuration used for Jest (in testing), and for the Webpack Dev Server itself.

기본적으로 config-overrides.js는 Webpack 설정을 커스터마이징하기 위한 단 하나만의 함수를 export한다. 하지만 이 파일에서 세 개의 함수로 구성된 객체를 export하는 것도 가능하다. 이 방법으로 Jest나 Webpack Dev Server 자체에 대한 커스터마이징도 가능해진다.

```js
// config-overrides.js

module.exports = {
  devServer: function (configFunction) {
    return function (proxy, allowedHost) {
      // Create the default config by calling configFunction with the proxy/allowedHost parameters
      const config = configFunction(proxy, allowedHost)

      config.client = {
        overlay: false,
      }

      return config
    }
  },
}
```

나는 개발 생산성의 향상을 위해 devServer의 overlay 속성을 false로 바꾸려고 했다. overlay가 true이면 앱 실행에 문제가 없는 warning이라도 경고창이 브라우저를 덮어버려 작업을 할 수가 없다. 따라서 무조건 모든 error와 warning이 해결된 상태여야만 앱 실행 모습을 확인할 수 있기 때문에 작업 속도가 늦어지게 된다.

커스터마이징 방법도 간단하다. 먼저 configFunction 함수를 호출해 기본 설정을 만든다. 기본 설정에 필요한 설정들을 추가해주고 반환해주면 끝!

## 😅마무리

가장 이상적인 개발자의 모습은 webpack, babel 등 빌드를 위한 모든 지식들을 완벽하게 갖추어 굳이 CRA를 쓰지 않고도 개발환경을 구축하는 것이라고 생각한다. 그래도 이런 꼼수 하나 제대로 알고 있으면, 필요할 때 좀 더 빠르게 개발환경을 구축할 수 있을 것이기에 알아둬서 나쁠 건 없다고 본다.

# :books:참고자료

https://www.npmjs.com/package/react-app-rewired

https://create-react-app.dev/docs/getting-started/
