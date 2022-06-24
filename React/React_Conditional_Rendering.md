# React

React 공식 문서를 보며 실습한 내용을 기록으로 남긴다. 실습 환경은 CRA로 만든 React 애플리케이션에 진행했음을 미리 밝힌다. 이번 문서의 예시에서 App 컴포넌트가 계속 등장할텐데, CRA의 프로젝트의 최상위 컴포넌트이기도 한 App 컴포넌트는 최종적으로 렌더링되는 컴포넌트라고 생각하면 된다.

## Docs - 조건부 렌더링

React에서 조건부 렌더링은 JavaScript에서의 조건 처리와 같이 동작한다. 예시를 통해 살펴보자.

```jsx
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

```jsx
function Greeting(props) {
	if (props.isLoggedIn) {
		return <UserGreeting></UserGreeting>
	} else {
		return <GuestGreeting></GuestGreeting>
	}
}
```

이 예시는 상위 컴포넌트에서 전달된 isLoggedIn prop에 따라 다른 인사말을 렌더링하고 있다. 이렇게 if 를 사용해서 조건부 렌더링을 하는 것도 좋은 방법이지만 실제로는 JSX 안에서 인라인으로 조건부 렌더링을 구현하는 경우가 훨씬 많다.

### 논리 && 연산자로 If 를 인라인으로 표현

JSX 안에서는 중괄호를 이용해서 표현식을 포함할 수 있다. 그 안에 Javascript의 논리 연산자 && 를 사용하면 쉽게 엘리먼트를 조건부로 넣을 수 있다.

```jsx
function Mailbox(props) {
	return (
		<div>
			<h1>Hello!</h1>
			{props.unreadMessages.length > 0 &&
				`You have ${props.unreadMessages.length} messages to read`}
		</div>
	)
}
```

```jsx
function App() {
	const messages = ['React', 'RE:React']
    // const messages = []
	return <Mailbox unreadMessages={messages}></Mailbox>
}
```

JavaScript에서 `true && expression`은 항상 `expression`으로 평가되고 `false && expression`은 항상 `false`로 평가된다. 따라서 `&&` 뒤의 엘리먼트는 조건이 `true`일 때 출력이 된다. 조건이 `false`라면 React는 무시하고 건너뛴다.

- messages의 길이가 0 이상일 때

![image-20220623113625757](md-images/image-20220623113625757.png)	

- messages의 길이가 0 이상이 아닐 때

![image-20220623113710955](md-images/image-20220623113710955.png)	

정상적으로 작동한다! 하지만 falsy 표현식을 반환하면 여전히 `&&` 뒤에 있는 표현식은 건너뛰지만 falsy 표현식이 반환된다는 것에 주의해야 한다. falsy 표현식이란 boolean 문맥에서 false로 평가되는 값들을 의미한다.

| 종류      | 설명                                                 |
| --------- | ---------------------------------------------------- |
| false     | -                                                    |
| 0         | -                                                    |
| -0        | -                                                    |
| 0n        | BigInt, boolean으로 사용되면 숫자와 같은 규칙을 따름 |
| ""        | 빈 문자열                                            |
| null      | 아무런 값도 없음                                     |
| undefined | 원시값                                               |
| NaN       | 숫자가 아님                                          |

```jsx
render() {
  const count = 0;
  return (
    <div>
      {count && <h1>Messages: {count}</h1>}
    </div>
  );
}
```

위의 경우에 `<div>0</div>`이 render 메서드에서 반환된다.

### 조건부 연산자로 If-Else 구문 인라인으로 표현하기

엘리먼트를 조건부로 렌더링하는 다른 방법은 조건부 연산자인 `condition ? true: false`를 사용하는 것이다. 앞의 로그인 예시를 다시 살펴보자.

```jsx
function App() {
	const [isLoggedIn, setIsLoggedIn] = useState(false)
	const handleBtnClick = () => {
		setIsLoggedIn(prev => !prev)
	}
	return (
		<div>
			<div>
				<button onClick={handleBtnClick}>
					{isLoggedIn ? 'Logout' : 'Login'}
				</button>
			</div>
			<div>
				<Greeting isLoggedIn={isLoggedIn}></Greeting>
			</div>
		</div>
	)
}
```

위 예시는 본문 앞에서 정의했던 Greeting 컴포넌트의 상위 컴포넌트 모습이다. 이 컴포넌트에서는 버튼의 텍스트를 조건부로 렌더링하도록 구현했다. 로그인이 되지 않았으면 Login을, 로그인 상태면 Logout을 렌더링한다. 중괄호를 사용하는 조건부 렌더링은 간단하지만 표현식이 너무 길어지면 가독성이 떨어지므로 주의해서 사용하자.

### 컴포넌트가 렌더링하는 것을 막기

가끔 상위 컴포넌트에 의해 렌더링되는 컴포넌트 자체를 숨기고 싶을 때가 있다. 이 때 렌더링 결과를 반환하는 대신 null을 반환하면 해결할 수 있다.

```jsx
function WarningBanner(props) {
	if (!props.warn) {
		return null
	}
	return <div>Warning!</div>
}
```

```jsx
function App() {
	const [showWarning, setShowWarning] = useState(false)
	const handleBtnClick = () => {
		setShowWarning(prev => !prev)
	}
	return (
		<div>
			<div>
				<WarningBanner warn={showWarning} />
				{/* {showWarning ? <WarningBanner /> : ''} */}
				<button onClick={handleBtnClick}>
					{showWarning ? 'hide' : 'show'}
				</button>
			</div>
		</div>
	)
}
```

개인적으로는 위 예시에서 각주 안에 작성한 방식을 더 선호하긴 한다. 

# :books:참고자료

https://ko.reactjs.org/docs/conditional-rendering.html