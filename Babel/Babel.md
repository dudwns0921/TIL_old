# Babel

## 기초

바벨(Babel)은 자바스크립트 컴파일러로, ECMA스크립트 2015+ 코드를 현재 및 이전 브라우저나 환경에서 하위 호환 버전의 자바스크립트로 변환하는 데 주로 사용되는 툴 체인이다.

## 사용방법

1. 설치

```shell
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

2. 프로젝트 내부에 설정 파일 생성

```json
{
  "presets": ["@babel/preset-env"],
}
```

@babel/preset-env는 대상 환경에 대한 세세한 설정 없이 최신 JavaScript를 사용할 수 있는 스마트 프리셋이다. 간단하게 정리하자면, @babel/preset-env은 대상 환경을 자동으로 인식하고 그에 매핑된 설정을 찾아 바벨로 전달한다.

3. 코드 변환

```shell
./node_modules/.bin/babel src --out-dir lib
or
npx babel src --out-dir
```

위 명령어로 src 폴더의 모든 코드들을 컴파일해 그 결과물을 lib 폴더에 저장한다.

## 개발 모드에서의 바벨 설정

위에서 알아본 사용 방법은 바벨로 변환한 코드가 결과물로 저장되는 방법이다. 하지만 우리가 개발할 때는 매번 결과물을 저장할 필요 없이 단순히 코드가 바벨을 적용시킨 상태로 실행되기만 하면 된다. 그래서 사용하는 것이 @babel/node와 nodemon이다. 

### @babel/node

babel-node는 Node.js CLI와 정확히 동일하게 작동하는 CLI로, 실행되기 전에 Babel presets 과 plugins 플러그인으로 컴파일한다.

### nodemon

nodemon은 디렉터리에서 파일 변경이 감지될 때 Node 기반 애플리케이션을 자동으로 재시작해 개발 생산성을 향상시켜주는 도구이다.

```json
//package.json

"scripts": {
  "dev": "nodemon --exec babel-node index.js"
},
```

babel-node와 nodemon을 사용해 dev라는 명령어를 만들었다. 

좀 더 자세히 설명해보자면,

1. nodemon 명령어로 index.js에 수정이 일어날 때마다 애플리케이션이 재시작된다.
2. 재시작하는 과정에서 babel-node 명령어로 인해 index.js에 대해 컴파일이 수행된다.

# :books:참고자료

노마드코더 강의 일부

https://babeljs.io/docs/en/babel-node

https://www.npmjs.com/package/nodemon