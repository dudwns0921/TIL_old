# React

## Docs - State 올바르게 사용하기

### 직접 state 수정 금지

예를 들어, 이 코드는 컴포넌트를 다시 렌더링하지 않는다.

```js
// Wrong
this.state.comment = 'Hello';
```

대신에 `setState()`를 사용해 state의 값을 수정한다.

```js
// Correct
this.setState({comment: 'Hello'});
```

`this.state`를 지정할 수 있는 유일한 공간은 바로 constructor이다.

### State 업데이트는 비동기적일 수 있다.

React는 성능을 위해 여러 setState() 호출을 단일 업데이트로 한꺼번에 처리할 수 있다. this.props와 this.state가 비동기적으로 업데이트될 수 있기 때문에 다음 state를 계산할 때 해당 값에 의존해서는 안 된다.

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

이를 수정하기 위해 객체보다는 함수를 인자로 사용하는 다른 형태의 `setState()`를 사용한다. 이 함수는 이전 state를 첫 번째 인자로 받고, 업데이트가 적용된 시점의 props를 두 번째 인자로 받는다.

```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

위에서는 화살표 함수를 사용했지만, 일반적인 함수에서도 동일하게 작동한다.

```js
// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

### State 업데이트는 병합된다.

`setState()`를 호출할 때 React는 인자로 전달된 객체를 현재 state로 병합한다. 예를 들어, state는 다양한 독립적인 변수를 포함할 수 있다.

```js
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
     };
  }
```

별도의 `setState()` 호출로 이러한 변수를 독립적으로 업데이트할 수 있다.

```js
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
        });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
        });
    });
  }
```

### 데이터는 아래로 흐른다.

컴포넌트는 자신의 state를 자식 컴포넌트에 props로 전달할 수 있다. 일반적으로 이를 하향식 혹은 단방향식 데이터 흐름이라고 한다. 모든 state는 항상 특정한 컴포넌트가 소유하고 있으며 그 state로부터 파생된 UI 또는 데이터는 트리 구조에서 오직 자신의 **아래**에 있는 컴포넌트에만 영향을 미친다.

# :books:참고자료

https://ko.reactjs.org/docs/state-and-lifecycle.html