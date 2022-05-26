# React

React 공식 문서를 보며 실습한 내용을 기록으로 남긴다. 실습 환경은 CRA로 만든 React 애플리케이션에 진행했음을 미리 밝힌다.

## Docs - Introducing JSX

### JSX란?

```jsx
const element = <h1>Hello, world!</h1>;
```

JSX란 Javascript를 확장한 문법으로, UI를 구현하기 위해 React와 함께 사용할 것이 권장된다. HTML과 비슷해 템플릿 언어가 떠오를 수도 있지만 Javascript의 모든 기능들이 포함되어 있다는 점을 알아두자.

### JSX에 표현식 포함하기

```react
// index.js

import React from 'react'
import ReactDOM from 'react-dom/client'

const name = 'Jun'
const element = <h1 className="greeting">Hello, {name}</h1>

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(<React.StrictMode>{element}</React.StrictMode>)
```

![image-20220526112947891](md-images/image-20220526112947891.png)	

JSX의 중괄호 안에는 유효한 모든 Javascript 표현식을 넣을 수 있다. 지금은 간단하게 변수를 가져오는 정도에 그쳤지만, 계산식이나 함수 사용도 가능하다.

#### JSX도 표현식

일반 Javascript로 컴파일되면 JSX는 Javascript 객체로 인식된다. 따라서 JSX는 다음과 같이 사용할 수 있다.

- if 구문 및 for loop 안에서 사용
- 변수로 할당
- 인자로서 사용
- 함수의 반환값으로 사용

```react
const user = {
  name: 'Jun',
  age: 28,
}

function getGreeting(user) {
  if (user) {
    return <h1>Hello, {user.name}!</h1>
  }
  return <h1>Hello, Stranger.</h1>
}

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(<React.StrictMode>{getGreeting(user)}</React.StrictMode>)
```

결과는 위와 동일하다.

#### JSX 속성 정의

JSX를 속성을 정의하는 방법에는 두 가지 방법이 있다.

- 따옴표를 사용해 문자열로 정의
- 중괄호를 사용해 Javascript 표현식 삽입

주의해야 할 점은 두 가지 방법을 동시에 사용해서는 안된다는 점이다.

```react
const aboutReact = {
  type: 'Framework',
  url: 'https://www.reactjs.org',
}

const element = (
  <a style={{ marginRight: 10 }} href="https://www.reactjs.org">
    리액트 링크
  </a>
)
const element2 = <a href={aboutReact.url}>리액트 링크</a>

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <React.StrictMode>
    {element}
    {element2}
  </React.StrictMode>
)
```

![image-20220526114857006](md-images/image-20220526114857006.png)	

두 element는 동일한 링크를 구현한다.

#### JSX는 객체를 표현

JSX는 Javascript를 확장한 문법이기에 브라우저가 바로 이해할 수 없어 일반 Javascript로 컴파일이 필요하다. 이 때 컴파일러는 JSX를 `React.createElement()` 호출로 컴파일한다. 따라서 아래 두 예시는 동일하다.

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
import React from 'react'

const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

그리고 React.createElement() 호출이 JSX 사용에 필수적이기 때문에 위와 같이 React가 Scope 안에 존재해야 한다. 

`React.createElement()`는 버그가 없는 코드를 작성하는 데 도움이 되도록 몇 가지 검사를 수행하며, 기본적으로 다음과 같은 객체를 생성한다.

```js
// 주의: 다음 구조는 단순화되었다.
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

이러한 객체를 “React 엘리먼트”라고 하며, 화면의 요소들을 나타내는 표현이라고 생각하면 된다. React는 이 객체를 읽어서, DOM을 구성하고 최신 상태로 유지하는 데 사용한다.

# :books:참고자료

https://ko.reactjs.org/docs/introducing-jsx.html