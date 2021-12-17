# Componenets와 Props

개념적으로 컴포넌트는 javascript 함수와 유사하다. 

props라는 임의의 입력을 받은 후 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환한다.



# 함수 컴포넌트와 클래스 컴포넌트

컴포넌트를 정의하는 데는 두 가지 방법이 있다.

```react
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}
```

이 함수는 데이터를 가진 하나의 props(속성을 나타내는 데이터) 객체 인자를 받은 후 React 엘리먼트를 반환한다.

javascript 함수이기 때문에 말 그대로 함수 컴포넌트라고 부른다.



```react
class Welcome extends React.Component {
	render() {
		return <h1>Hello, {this.props.name}</h1>;
	}
}
```

react 관점에서 볼 때 위 두 가지 유형의 컴포넌트는 동일하다.



## :bulb:Tip!

사용자 정의 컴포넌트의 이름은 항상 대문자로 시작해야 한다.



# 컴포넌트 렌더링

React 엘리먼트는 사용자 정의 컴포넌트로도 나타낼 수 있다.

```react
const element = <Welcome name="Jun" />
```

React가 사용자 정의 컴포넌트를 발견하면 JSX 어트리뷰트(여기서는 name)과 자식을 해당 컴포넌트의 단일 객체로 전달한다.

이 객체를 props라고 한다.



다음은 페이지에 Hello, Jun을 렌더링하는 예시이다.

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Jun" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

이 예시에서는 다음과 같은 일들이 일어난다.

1. `<Welcome name="Jun" />` 엘리먼트로 `ReactDOM.render()`를 호출한다.
2. React는 `{name: 'Sara'}`를 props로 하여 `Welcome` 컴포넌트를 호출한다.
3. `Welcome` 컴포넌트는 결과적으로 `<h1>Hello, Jun</h1>` 엘리먼트를 반환합니다.
4. React DOM은 `<h1>Hello, Jun</h1>` 엘리먼트와 일치하도록 바뀐 부분만을 찾아 DOM을 효율적으로 업데이트한다.



# 컴포넌트 합성

컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할 수 있다.

이는 모든 세부 단계에서 동일한 추상 컴포넌트를 사용할 수 있음을 의미한다.



예시로 이를 살펴보자.

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Jun" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

이런 식으로 Welcome을 여러 번 업데이트하는 App 컴포넌트를 만들 수 있다. 



# 컴포넌트 추출

```react
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

이 컴포넌트는 `author`(객체), `text`(문자열) 및 `date`(날짜)를 props로 받은 후 소셜 미디어 웹 사이트의 코멘트를 나타낸다.

이 컴포넌트는 구성요소들이 모두 중첩 구조로 이루어져 있어서 변경하기 어렵고, 각 구성요소를 개별적으로 재사용하기도 힘들다.

```react
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}

function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

Avatar 컴포넌트와 UserInfo 컴포넌트를 추출했다.

```react
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Comment 컴포넌트가 훨씬 간단해진 것을 확인할 수 있다.

하지만 재사용 가능한 컴포넌트를 만들어 놓는 것은 더 큰 앱에서 작업할 때  큰 도움이 된다.

UI 일부가 여러 번 사용되거나 UI 일부가 자체적으로 복잡한 경우에는 별도의 컴포넌트로 만드는 게 좋다.



## :bulb:Tip!

### props는 읽기 전용이다.

즉, 함수 컴포넌트나 클래스 컴포넌트 모두 컴포넌트의 자체 props를 수정해서는 안 된다는 의미이다.

### PropTypes

PropTypes는 React에서 타입 체크를 위해 사용되는 라이브러리이다.

```react
Welcome.propTypes = {
	name: PropTypes.string,
}
```

위와 같은 경우 Welcome 컴포넌트의 name prop은 string 타입을 받아야 한다.

이러한 경고 메시지는 개발 모드에서만 출력되기 때문에 이를 위반한다고 해서 사용자들에게는 아무 영향을 끼치지 않는다.



# :books:참고자료

https://ko.reactjs.org/docs/components-and-props.html

노마드코더 강의
