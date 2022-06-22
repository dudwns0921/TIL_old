# Javascript

## CommonJS, AMD, 그리고 UMD란?

모듈이란 1개 이상의 값(객체, 함수, 변수 등)을 내보내는 자바스크립트 파일을 말한다. 모던 웹 개발에서는 자바스크립트가 차지하는 비중이 무척 높아졌기 때문에 모듈을 통한 기능과 관심사의 분리는 필수적이다. 자바스크립트 모듈 선언 포맷에는 AMD와 CommonJS 가 있다.

## AMD(Asynchronous Module Definition)

AMD는 모듈과 그것이 의존하고 있는 모듈도 함께 명시해 둘을 비동기적으로 불러올 수 있는 방법을 명시하고 있다. 이는 동기적 로딩이 성능, 사용성 등의 문제들을 초래할 수 있는 브라우저 환경에 적합하다. 간단한 예시를 살펴보자.

```js
define(['jquery'], function ($) { // 의존 모듈로 jQuery를 선언
	// 함수 선언	
	function withjQuery(){};
	
  // 외부에 함수를 노출시켜 사용할 수 있도록 한다.
  return withjQuery; 
});
```

AMD를 사용하면 실행할 코드가 필요한 파일만 가져오기 때문에 성능 향상을 기대할 수 있다. 그리고 의존하고 있는 모듈을 확실히 가져온 다음에 코드를 실행할 수 있기 때문에 오류도 줄어든다. AMD 포맷을 구현한 대표적인 라이브러리에는 Require.js, Dojo Toolkit 등이 있다.

## CommonJS

 CommonJS는 웹 브라우저가 아닌 환경에서의 자바스크립트 모듈 포맷을 확립하기 위한 프로젝트다. 이 프로젝트가 시작된 가장 큰 이유는 서로 다른 개발 환경(웹 서버, 네이티브 데스크탑 앱 등)에서 만들어진 자바스크립트 코드를 공유하기 위한 포맷이 없었기 때문이었다.

 2009년 ServerJS라는 이름의 프로젝트로 시작되었고 이후 CommonJS로 이름이 바뀌게 된다. 자바스크립트의 표준이 되는 ECMAScript를 정의하는 ECMA 국제 그룹 TC39와 직접적인 관계는 없지만, TC39의 멤버 중 일부는 CommonJS 프로젝트에 참여하기도 했다.

CommonJS 포맷은 Node.js에서 구현했기 때문에 AMD보다 접해본 사람이 더 많을 것이다. 앞서 작성한 `withJquery` 모듈을 CommonJS 형식으로 작성하면 아래와 같다.

```js
// 의존 모듈 선언 
var $ = require('jquery');

// 함수 선언	
function withjQuery(){};

// 외부에 함수를 노출시킨다.
module.exports = withjQuery;
```

## UMD(Universal Module Definition)

앞에서 제시한 예제 코드를 통해 AMD와 CommonJS 포맷 모두 ‘의존 모듈을 정의하고 새로운 모듈을 내보낸다’는 같은 목적을 가지고 있다는 것을 알 수 있다. 다만 그 방식이 다를 뿐이다.

그런데 자바스크립트로 개발을 하다 보면 두 방식을 모두 지원해야 하는 상황이 생긴다. 대표적으로 오픈소스 라이브러리 모듈은 다양한 환경을 지원해야 할 필요가 있을 것이다. 그럴 때는 UMD를 사용해서 모듈을 선언할 수 있다.

UMD는 CommonJS처럼 스펙이 아닌 코드 작성 패턴이라고 보면 되며, 다양한 패턴이 존재한다.

> [umdjs/umd](https://github.com/umdjs/umd)

위의 저장소에서 필요한 패턴을 참조해 사용하면 될 것이다.

# :books:참고자료

https://github.com/amdjs/amdjs-api/blob/master/AMD.md

https://blog.rhostem.com/posts/2019-06-23-universal-module-definition-pattern