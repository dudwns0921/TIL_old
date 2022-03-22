# Effect Hook

state가 변경될 때 react는 컴포넌트를 렌더링하기 위해 모든 코드들을 다시 실행한다.

하지만 코드들 중 한 번만 실행해도 되는 코드가 있는 경우에는 어떻게 해야할까?

예를들어, API로 외부 데이터를 가져올 때 컴포넌트 처음 렌더링되는 그 순간에만 API 요청을 하고,

이후에 state가 변화할 때는 그 API에서 똑같은 정보를 가져오고 싶지는 않을 것이다.

이 때 사용하는 것이 바로 Effect Hook이다. 

useEffect는 실행시키고자 하는 함수와 React가 이벤트를 주시하게끔 하는 dependency로 이루어져있다.

즉, 내가 원하는 부분을 지정하여 그 부분만을 변화시킬 수 있는 것이다.

```react
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Hi!")
  }, []);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

<img src="./md-images/2조_정영준 2021-12-16 08-19-29-558-16396104051031.jpg" alt="2조_정영준 2021-12-16 08-19-29-558" style="zoom: 67%;" />

- **6번째 줄**

useEffect 함수에 dependency가 비어있다.

이는 컴포넌트가 렌더링되는 처음 그 순간만 코드가 실행되고 그 다음부터는 state가 바뀌어도 코드는 실행되지 않음을 뜻한다.

따라서 버튼을 눌러 count state의 값을 아무리 증가시켜도 Hi는 콘솔에 한 번만 출력된다.

만약 dependency 안에 count state가 들어있다면 count가 바뀔 때마다 useEffect 안의 코드는 계속 실행될 것이다.

useEffect에는 유용한 기능이 하나 더 있는데, 바로 cleanup이라는 기능이다.

cleanup은 컴포넌트가 없어질 때 코드를 실행시킬 수 있도록 한다.

자세한 건 아래 예시를 통해 살펴보자.

```react
import { useState, useEffect } from "react";

function Hello() {

  useEffect(() => {
    console.log("hi :)");
    return () => console.log("bye :(");
  }, []);
  return <h1>Hello</h1>;
}

function App() {
  const [showing, setShowing] = useState(false);
  const onClick = () => setShowing((prev) => !prev);
  return (
    <div>
      {showing ? <Hello /> : null}
      <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}
```

<img src="./md-images/2조_정영준 2021-12-16 08-21-35-586-16397024114401.jpg" alt="2조_정영준 2021-12-16 08-21-41-176" style="zoom:67%;" />    

<img src="./md-images/2조_정영준 2021-12-16 08-21-41-176-16396105733182.jpg" alt="2조_정영준 2021-12-16 08-21-41-176" style="zoom:67%;" />    

버튼을 누르면 Hello 컴포넌트가 생성되고, 다시 버튼을 누르면 Hello 컴포넌트가 없어지는 간단한 구조이다.

컴포넌트가 렌더링될 때 콘솔에 Hi가 출력되고 없어질 때 bye가 출력된다.

- **5번째 줄**

기존의 Effect 함수를 사용했던 것과 다르게 return에 또 다른 함수가 들어있는 것을 확인할 수 있다.

컴포넌트가 없어질 때 실행시킬 함수를 return에다가 작성하면 cleanup 기능을 사용할 수 있다.

# :books:참고자료

노마드코더 강의
