# Effect Hook

state가 변경될 때 react는 모든 code들을 항상 다시 실행한다.

하지만 code들 중 한 번만 실행해도 되는 code가 있는 경우에는 어떻게 해야할까?

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



