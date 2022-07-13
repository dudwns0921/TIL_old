# React

React 공식 문서를 보며 실습한 내용을 기록으로 남긴다. 실습 환경은 CRA로 만든 React 애플리케이션에 진행했음을 미리 밝힌다.

## Docs - Form

> 대부분의 경우, JavaScript 함수로 폼의 제출을 처리하고 사용자가 폼에 입력한 데이터에 접근하도록 하는 것이 편리합니다. 이를 위한 표준 방식은 “제어 컴포넌트 (controlled components)“라고 불리는 기술을 이용하는 것입니다.

### 제어 컴포넌트와 비제어 컴포넌트

React를 처음 배울 때 아무런 고민 없이 제어 컴포넌트를 사용했던 기억이 있다. 당시에는 'React에서는 input을 처리할 때 이렇게 하는구나.' 정도로 쉽게 받아들였다. 그래서 지금도 '왜 그렇게 하는데?' 라는 질문을 받는다면 아마 대답조차 어려울 것 같다. React에서는 왜 제어 컴포넌트 방식을 사용할까?

먼저 각각의 정의를 살펴보자.

- 제어 컴포넌트 : React에 의해 값이 제어되는 Form 엘리먼트를 반환하는 컴포넌트
- 비제어 컴포넌트 : 일반적인 Form 엘리먼트를 반환하는 컴포넌트로 ref를 통해 input의 값에 접근한다.

컴포넌트라는 용어에 너무 집중하기보다는 방식이라고 이해하는 것이 개인적으로는 이해하기 더 쉽다고 생각한다.

#### 비제어 컴포넌트

```react
import React from 'react'
import { useRef } from 'react'

function App() {
	const inputRef = useRef()
	const handleSubmit = e => {
		e.preventDefault()
		console.log(`Your name is ${e.target.value}`)
	}
	return (
		<form onSubmit={handleSubmit}>
			<input ref={inputRef} type="text" />
			<input type="submit" value="Submit" />
        </form>
	)
}

export default App
```

위는 비제어 컴포넌트 예시이다. 실행되는 순서는 다음과 같다.

1. 값을 input에 입력한다.
2. submit 버튼을 누르면 ref를 통해 그 값을 가져와 콘솔에 출력한다.

#### 제어 컴포넌트

제어 컴포넌트에서는 input이 자신의 값을 prop으로 받아들인다. 그리고 그 값을 변경시키기 위한 콜백 함수도 받는다. 이 방식이 좀 더 React 스럽다고 이야기할 수 있을 것이다.

```react
import React from 'react'
import { useState } from 'react'

function App() {
	const [name, setName] = useState('')
	const handleSubmit = e => {
		e.preventDefault()
		console.log(`Your name is ${name}`)
	}
	const handleInputChange = e => {
		setName(e.target.value)
		console.log(`name value: ${name}`)
	}
	return (
		<form onSubmit={handleSubmit}>
			<input onChange={handleInputChange} value={name} type="text" />
			<input type="submit" value="Submit" />
		</form>
	)
}

export default App
```

실행되는 순서는 다음과 같다.

1. input에 새로운 값을 입력한다.
2. onChange에 전달한 함수가 실행되고 name state에 새로운 값이 설정된다.
3. submit 버튼을 누르면 name state의 값을 콘솔에 출력한다.

#### 차이점

둘의 차이는 결국 값을 pull하는지, push하는지에 달렸다고 생각한다.  `<input>`, `<textarea>`, `<select>`와 같은 폼 엘리먼트는 일반적으로 사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트한다. 비제어 컴포넌트에서는 폼 엘리먼트가 자체적으로 업데이트한 값을 필요한 순간에 꺼내서 사용하는 것이다.

이와 반대로 제어 컴포넌트는 React의 state를 이용해 갱신된 값을 폼 엘리먼트에 계속 push한다. 그렇기 때문에 UI와 실제 값이 동기화된다는 특징을 가진다. 이는 제어 컴포넌트가 폼 엘리먼트의 값의 변화에 즉각적으로 대응할 수 있다는 의미이기도 하다.

#### 그래서 언제 사용해야해?🤔

아래의 표를 참고해서 자신의 상황과 맞는 방식을 선택하자.

| 상황                                                         | 비제어 컴포넌트 | 제어 컴포넌트 |
| ------------------------------------------------------------ | --------------- | ------------- |
| one-time value retrieval (e.g. on submit)                    | ✅               | ✅             |
| [validating on submit](https://goshacmd.com/submit-time-validation-react/) | ✅               | ✅             |
| [instant field validation](https://goshacmd.com/instant-form-fields-validation-react/) | ❌               | ✅             |
| [conditionally disabling submit button](https://goshacmd.com/form-recipe-disable-submit-button-react/) | ❌               | ✅             |
| enforcing input format                                       | ❌               | ✅             |
| several inputs for one piece of data                         | ❌               | ✅             |
| [dynamic inputs](https://goshacmd.com/array-form-inputs/)    | ❌               | ✅             |

___

### 다중 입력 제어하기

여러 input 엘리먼트를 제어해야 할 때, 각 엘리먼트에 name 어트리뷰트를 추가하고 event.target.name 값을 통해 핸들러가 어떤 작업을 할 지 선택할 수 있게 해준다.

```react
import React from 'react'

class App extends React.Component {
	constructor(props) {
		super(props)
		this.state = {
			name: '',
			age: 0,
		}
		this.handleInputChange = this.handleInputChange.bind(this)
		this.handleSubmit = this.handleSubmit.bind(this)
	}
	handleInputChange = e => {
		const target = e.target
		const value = target.value
		const name = target.name
		this.setState({
			[name]: value,
		})
		console.log(this.state.name, this.state.age)
	}
	handleSubmit = e => {
		e.preventDefault()
		console.log(
			`Your name is ${this.state.name} and your age is ${this.state.age}`,
		)
	}
	render() {
		return (
			<form onSubmit={this.handleSubmit}>
				<input
					name="name"
					type="text"
					onChange={this.handleInputChange}
					value={this.state.name}
				/>
				<input
					name="age"
					type="text"
					onChange={this.handleInputChange}
					value={this.state.age}
				/>
				<input type="submit" value="Submit" />
			</form>
		)
	}
}

export default App
```

먼저 해당 방식을 사용하기 위해서는 클래스형 컴포넌트를 생성해야 한다. 왜냐하면 일반적으로 함수형 컴포넌트에서 사용하는 useState는 state와 그 state만을 갱신하는 setState 함수를 반환하기 때문이다.

추가적으로 주어진 input 태그의 name에 일치하는 state를 업데이트하기 위해 ES6의 [computed property name](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer#속성_계산명) 구문을 사용하고 있다.

# :books:참고자료

https://ko.reactjs.org/docs/forms.html#controlled-components

https://goshacmd.com/controlled-vs-uncontrolled-inputs-react/