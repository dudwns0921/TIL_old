# React State Hook

Hook은 React에서 제공하는 특별한 함수로 그 중에는 이번에 알아볼 useState가 포함된다.



이전에 다음과 같은 의문이 든 적이 있다.

웹페이지의 일부분에만 새로운 콘텐츠들을 받아오려면 어떻게 해야하지?

새로운 콘텐츠들을 받아올 때마다 전체 페이지를 갱신하는 것은 엄청난 시간과 자원 낭비일 것이다.

이에 대한 해결책으로 Ajax를 배웠던 적이 있는데, useState를 공부하면서 Ajax와 굉장히 유사하다는 느낌을 받았다.



useState 함수는 state 변수와 해당 변수의 값을 갱신할 수 있는 함수를 제공한다.

modifier 함수를 사용해 state, 즉 어플리케이션의 데이터를 바꿀 때 컴포넌트는 새로운 값을 가지고 다시 한 번 렌더링된다.

이게 바로 리액트가 제공하는 가장 중점적인 기능이다.

데이터가 바뀔 때마다 컴포넌트를  리렌더링하고 UI를 갱신하는 것이다.

여기서 React의 진가가 발휘되는데, React는 기존의 컴포넌트와 달라진 부분만을 확인해 그 부분만 갱신이 이루어지게 한다.



아래 예시를 통해 실제로 useState 함수가 어떻게 쓰이는지 알아보자.

```react
 1:  import React, { useState } from 'react';
 2:
 3:  function Example() {
 4:    const [count, setCount] = useState(0);
 5:
 6:    return (
 7:      <div>
 8:        <p>You clicked {count} times</p>
 9:        <button onClick={() => setCount(count + 1)}>
10:         Click me
11:        </button>
12:      </div>
13:    );
14:  }
```

- **네 번째 줄**

`useState`를 이용해서 state 변수와 해당 state를 갱신할 수 있는 함수를 만들었다.

또한, `useState`의 인자의 값으로 `0`을 넘겨주면 `count` 값을 0으로 초기화할 수 있다.

- **아홉 번째 줄**

사용자가 버튼 클릭을 하면 `setCount` 함수를 호출하여 state 변수를 갱신한다.

React는 새로운  count변수를 `Example` 컴포넌트에 넘기며 해당 컴포넌트를 리렌더링한다.



여기서 `count+1` 대신 `(current) => current + 1)` 와 같이 적을 수 있다.

이전 값을 바탕으로 현재의 값을 설정해주고 싶다면 함수를 사용하는 것이 더 안전하다. 



## :bulb:Tip!

### 최상위(at the Top Level)에서만 Hook을 호출해야 한다.

반복문, 조건문 혹은 중첩된 함수 내에서 Hook을 호출하면 안 된다.

대신 early return이 실행되기 전에 항상 React 함수의 최상위(at the top level)에서 Hook을 호출해야 한다.



### 오직 React 함수 내에서 Hook을 호출해야 한다.

Hook을 일반적인 JavaScript 함수에서 호출하면 안 된다.



# :books:참고자료

https://ko.reactjs.org/docs/hooks-state.html

노마드코더 강의
