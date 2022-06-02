# React

React 공식 문서를 보며 실습한 내용을 기록으로 남긴다. 실습 환경은 CRA로 만든 React 애플리케이션에 진행했음을 미리 밝힌다.

## Docs - 이벤트 처리하기

React 엘리먼트에서 이벤트를 처리하는 방식은 DOM 엘리먼트에서 이벤트를 처리하는 방식과 매우 유사하다. 몇 가지 문법 차이는 다음과 같다.

- React의 이벤트는 소문자 대신 캐멀 케이스(camelCase)를 사용한다.
- JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러를 전달한다.
- 이벤트 핸들러가 false를 반환해도 기본 동작을 방지할 수 없어 preventDefault를 명시적으로 호출해야 한다.

> https://stackoverflow.com/questions/128923/whats-the-effect-of-adding-return-false-to-a-click-event-listener

위 링크에서 이벤트 핸들러가 false를 반환하는 것이 어떤 의미인지 확인할 수 있다.

예시를 통해 차이를 살펴보자.

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

```jsx
<button onClick={activateLasers}>  Activate Lasers
</button>
```

먼저 HTML의 이벤트는 `onclick`이지만 React의 이벤트는 onClick으로 캐멀 케이스를 사용한 것을 확인할 수 있다. 또한 이벤트 속성의 값으로 HTML은 문자열을, React는 함수 그 자체를 전달하고 있다.

```react
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

submit으로 발생하는 새로고침을 방지하기 위해 preventDefault 함수를 명시적으로 호출하고 있다.

### 실제 사용해보기

```react
import React from 'react'

class App extends React.Component {
	constructor(props) {
		super(props)
		this.state = { isToggleOn: true }
        
		this.handleClick = this.handleClick.bind(this)
	}

	handleClick() {
		this.setState(prevState => ({
			isToggleOn: !prevState.isToggleOn,
		}))
	}

	render() {
		return (
			<button onClick={this.handleClick}>
				{this.state.isToggleOn ? 'ON' : 'OFF'}
			</button>
		)
	}
}

export default App
```

전체적인 과정 자체는 복잡하지 않다.

1. 버튼을 클릭하면 handleClick 메서드가 실행됨
2.  handleClick 메서드에서 setState를 사용해 isToggleOn의 값을 현재 isToggleOn 값의 반대값으로 수정
3. isToggleOn의 값에 따라 버튼에 노출되는 문자열의 값이 달라짐

주목해서 볼 만한 부분은 아래와 같다.

1. setState의 인자
2. 클래스 메서드 바인딩

#### setState의 인자

```react
	handleClick() {
		this.setState(prevState => ({
			isToggleOn: !prevState.isToggleOn,
		}))
	}
```

setState는 객체 혹은 함수를 인자로 사용할 수 있다. 함수를 사용할 경우, 이전 state를 첫 번째 인자로 받는다. 위 코드에서는 prevState라고 했지만 어떤 문자열이 들어가도 동일하게 이전 state를 받는다.

#### 클래스 메서드 바인딩

```react
class App extends React.Component {
	constructor(props) {
		super(props)
		this.state = { isToggleOn: true }
        
		this.handleClick = this.handleClick.bind(this)
	}
```

 JavaScript에서 클래스 메서드는 기본적으로 바인딩되어 있지 않다. `this.handleClick`을 바인딩하지 않고 `onClick`에 전달하였다면, 함수가 실제 호출될 때 `this`는 `undefined`가 된다.

이는 React만의 특수한 동작이 아니며, JavaScript에서 함수가 작동하는 방식의 일부이다. 일반적으로 `onClick={this.handleClick}`과 같이 뒤에 `()`를 사용하지 않고 메서드를 참조할 경우, 해당 메서드를 바인딩 해야 한다.

## :bulb:Bind()

메서드를 바인딩하는 이유는 바로 납득이 갔지만, 바인딩을 위해 작성된 구문이 잘 이해가 가지 않았다. 그래서 bind()에 대해 좀 더 알아보고자 했다.

```js
var foo = {
    x: 3
}

var bar = function(){
    console.log(this.x);
}

bar(); // undefined

var boundFunc = bar.bind(foo);

boundFunc(); // 3
```

위의 예시를 보면 객체 내부의 메서드가 아닌 일반 함수에서 this를 호출하고 있다. 그렇기 때문에 최상위 객체인 window를 참조하게 되고, window에는 변수 x가 정의되지 않았기 때문에 undefined를 출력한다.

원하는 결과를 얻기 위해서는 bar라는 함수가 foo객체를 참조해야 하는데 이 때 bind()를 사용한다. bind() 메서드는 함수를 생성하는데, 전달받은 첫 번째 인자로 this를 설정한다. 위에서는 foo객체가 this로 설정된 것이다.

```js
import React from 'react'

class App extends React.Component {
	constructor(props) {
		super(props)
		this.state = { isToggleOn: true }
        
		this.handleClick = this.handleClick.bind(this)
	}

	handleClick() {
		this.setState(prevState => ({
			isToggleOn: !prevState.isToggleOn,
		}))
	}
```

다시 위의 클래스형 컴포넌트 App을 살펴보자. 앞에서도 언급했지만 JavaScript에서 클래스 메서드는 기본적으로 바인딩되어 있지 않다. 따라서 우리가 해야할 일은 handleClick 메서드의 this를 App 클래스를 통해 생성되는 인스턴스로 설정하는 것이다.

이를 위해 bind() 메서드를 사용하고 첫 번째 인자로 this를 전달한다. 왜냐하면 클래스 내부에서의 this는 클래스의 인스턴스를 참조하기 때문이다.

# :books:참고자료

https://ko.reactjs.org/docs/handling-events.html

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/bind