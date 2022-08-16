# ✅Node.js와 npm이란?

 클라이언트 사이드, 즉 브라우저에서 동작하는 간단한 웹 애플리케이션은 브라우저만으로도 개발할 수 있다. 하지만 프로젝트 규모가 커짐에 따라 React, Angular 같은 프레임워크 또는 라이브러리를 도입하거나 Babel, Webpack 등 여러 가지 도구를 사용할 필요가 있다. 이 때 Node.js와 npm이 필요하다.

## Node.js

 2009년, 라이언 달이 발표한 Node.js는 크롬 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경이다. 간단히 말하자면 브라우저에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서 동작시킬 수 있는 자바스크립트 실행 환경이 Node.js이다.

### Node.js 특징

- Node.js는 자바스크립트를 사용해 개발

- Front-end와 Back-end에서 자바스크립트를 사용할 수 있다는 동형성은 별도의 언어 학습 시간을 단축

- Node.js는 **Non-blocking I/O와 단일 스레드 이벤트 루프**를 통한 높은 Request 처리 성능

> 데이터베이스로부터 대량의 데이터를 가져와 웹페이지에 표시할 때, 일반적으로 데이터베이스 처리에 대기시간(blocking)이 발생하기 때문에 웹페이지 표시가 지연되는 현상이 발생한다. 
> 
> Node.js의 모든 API는 비동기 방식으로 동작하여 Non-blocking I/O가 가능하고 단일 스레드 이벤트 루프 모델을 사용하여 보다 가벼운 환경에서도 높은 Request 처리 성능을 가지고 있다.

- Node.js는 데이터를 실시간 처리하여 빈번한 I/O가 발생하는 SPA(Single Page Application)에 적합

- CPU 사용률이 높은 애플리케이션에는 권장하지 않음

- Node.js에는 [Socket.io](https://poiemaweb.com/nodejs-socketio)라는 실시간 통신을 실현하는 라이브러리를 사용할 수 있어서 대량의 데이터 처리와 실시간 통신을 구현할 수 기능을 갖추고 있음

### Blocking I/O와 Non-blocking I/O란?

#### Blocking I/O(쓰레드 기반 동기방식)

- 하나의 쓰레드가 request를 받으면 모든 처리가 완료될때까지 기다리다가 처리결과가 완료되면 다시 응답을 보냄
- 기존 업무 처리가 완료되기 전에 또다른 request가 있으면 새로운 쓰레드가 업무를 처리함.
- 동시 request가 많은 경우 많은 쓰레드가 필요하게 되어 서버 과부하

#### Non-Blocking I/O(단일쓰레드 이벤트 루프 기반 비동기방식)

- 하나의 쓰레드가 request를 받으면 바로 다음 처리에 요청을 보내놓고 다른 작업을 처리하다가 먼저 요청한 작업이 끝나면 이벤트를 받아서 응답을 보냄
- 동시 request가 오더라도 처리가 완료될때까지 기다리지 않아도 되기 때문에 서버 부하가 적음

## NPM

 npm은 자바스크립트 패키지 매니저이다. Node.js에서 사용할 수 있는 모듈들을 패키지화해서 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI를 제공한다. 자신이 작성한 패키지를 공개할 수도 있고 필요한 패키지를 검색해 재사용할 수도 있다. 

#### NPM 장점

- 패키지 버전과 의존성 관리 용이

- 패키지명만 알고 있다면 쉬운 설치가 가능

#### npm init

명령어가 실행되면 package.json 파일이 생성되며 node 프로젝트가 생성된다. 이 때 node 프로젝트와 관련된 여러 정보들을 입력해주어야 하는데 아래 명령어를 입력하면 가장 기본적인 구조로 npm 프로젝트가 생성된다.

> npm init -y

#### npm install (패키지명)

패키지를 설치하는 명령어이다.

## :bulb:Tip

패키지는 라이브러리(library)와 유사한 개념으로 라이브러리가 코드의 작성을 위해 사용되는 코드의 묶음이라면, 패키지는 코드의 배포를 위해 사용되는 코드의 묶음이다.  
따라서 패키지는 경우에 따라 라이브러리를 포함할 수도 있으며, 일반적으로 라이브러리나 실행 파일(executable)을 포함한다.

정리하자면 패키지 매니저를 통해 패키지를 다운받고, 해당 패키지의 라이브러리를 실제 프로젝트에서 사용한다고 표현하는 것이 맞다고 생각한다.

#### NPM 지역 설치

```
npm install jquery --save-prod
```

그리고 지역 설치 명령어의 경우 명령어 옵션으로 `--save-prod`를 붙이지 않아도 동일한 효과가 난다. 또한, `install` 대신 `i`를 사용해도 된다.

```
# 위 명령어와 동일한 효과
npm i jquery
```

#### NPM 지역 설치 경로

위 명령어로 패키지를 설치하면 해당 프로젝트의 `node_modules` 라는 폴더가 생깁니다. 그리고 그 폴더 아래에 해당 패키지가 설치되어 있는 것을 확인할 수 있다.

#### NPM 전역 설치

NPM 전역 설치는 위와 같이 프로젝트 레벨이 아니라 시스템 레벨에서 사용할 자바스크립트 패키지를 설치할 때 사용한다.

```
npm install gulp --global
```

전역 설치 명령어 옵션 `--global` 대신 `-g`를 사용해도 된다.

#### NPM 전역 설치 경로

이렇게 설치된 자바스크립트 라이브러리는 어느 위치에서 해당 명령어를 실행했던지 간에 OS별로 아래와 같은 폴더 경로에 설치된다.

```
# window
%USERPROFILE%\AppData\Roaming\npm\node_modules

# mac
/usr/local/lib/node_modules
```

#### NPM 지역 설치 옵션 2가지

NPM 지역 설치에 자주 사용되는 2가지 옵션은 다음과 같다.

```
npm install jquery --save-prod
npm install jquery --save-dev
```

위 명령어는 아래와 같이 각각 축약할 수 있다.

```
npm i jquery
npm i jquery -D
```

여기서 설치 옵션에 아무것도 넣지 않은 `npm i jquery`는 `package.json`의 `dependencies`에 등록된다.

```
// package.json
{
  "dependencies": {
    "jquery": "^3.4.1"
  }
}
```

설치 옵션으로 `-D`를 넣은 경우에는 해당 라이브러리가 `package.json`의 `devDependencies`에 등록된다.

```
// package.json
{
  "devDependencies": {
    "jquery": "^3.4.1"
  }
}
```

#### 개발용 라이브러리와 배포용 라이브러리 구분하기

NPM 지역 설치를 할 때는 해당 라이브러리가 배포용(dependencies)인지 개발용(devDependencies)인지 꼭 구분해주어야 한다다. 예를 들어, `jquery`와 같이 화면 로직과 직접적으로 관련된 라이브러리는 배포용으로 설치해야 한다.

```
# 배포용 라이브러리 설치
npm i jquery
```

```
{
  "dependencies": {
    "jquery": "^3.4.1"
  }
}
```

이렇게 설치된 배포용 라이브러리는 `npm run build`로 빌드를 하면 최종 애플리케이션 코드 안에 포함된다.

그런데 만약 반대로 설치 옵션에 `-D`를 주었다면 해당 라이브러리는 빌드하고 배포할 때 애플리케이션 코드에서 빠지게 됩니다. 따라서, 최종 애플리케이션에 포함되어야 하는 라이브러리는 `-D`로 설치하면 안 된다. 개발할 때만 사용하고 배포할 때는 빠져도 좋은 라이브러리의 예시는 다음과 같다.

- `webpack` : 빌드 도구
- `eslint` : 코드 문법 검사 도구
- `imagemin` : 이미지 압축 도구

# :books:참고자료

이웅모, 모던 자바스크립트 Deep Dive, 위키북스, 2020

[Node.js Basics | PoiemaWeb](https://poiemaweb.com/nodejs-basics)

[[Node.js 1강]node js 란? 장점, 단점, 어떤 웹서비스에 사용해야할까?](https://junspapa-itdev.tistory.com/3?category=781922)

인프런 웹팩 가이드 강의
