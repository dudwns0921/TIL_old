# React State Hook

Hook은 특별한 함수이다.

예를 들어 `useState`는 state를 함수 컴포넌트 안에서 사용할 수 있게 해준다.

함수 컴포넌트를 사용하던 중 state를 추가하고 싶을 때 클래스 컴포넌트로 바꾸곤 했을 것이다.

하지만 이제 함수 컴포넌트 안에서 Hook을 이용하여 state를 사용할 수 있다.



`useState`를 호출하면 state 변수를 선언할 수 있다.

일반적으로 일반 변수는 함수가 끝날 때 사라지지만, state 변수는 React에 의해 사라지지 않는다.



`useState()`Hook의 인자로 넘겨주는 값은 state의 초기 값이다.

함수 컴포넌트의 state는 클래스와 달리 객체일 필요는 없고, 숫자 타입과 문자 타입을 가질 수 있습니다.

2개의 다른 변수를 저장하기를 원한다면 `useState()`를 두 번 호출해야 합니다.



`useState`는 state 변수, 해당 변수를 갱신할 수 있는 함수 이 두 가지 쌍을 반환한다.



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

`useState` Hook을 이용하면 state 변수와 해당 state를 갱신할 수 있는 함수가 만들어진다.

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
