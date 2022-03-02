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

## npm

 npm은 자바스크립트 패키지 매니저이다. Node.js에서 사용할 수 있는모듈들을 패키지화해서 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 CLI를 제공한다. 자신이 작성한 패키지를 공개할 수도 있고 필요한 패키지를 검색해 재사용할 수도 있다. 

# 

# :books:참고자료

이웅모, 모던 자바스크립트 Deep Dive, 위키북스, 2020

[Node.js Basics | PoiemaWeb](https://poiemaweb.com/nodejs-basics)

[[Node.js 1강]node js 란? 장점, 단점, 어떤 웹서비스에 사용해야할까?](https://junspapa-itdev.tistory.com/3?category=781922)


