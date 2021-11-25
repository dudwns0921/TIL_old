# React Rerendering

JSX 변수 사용법

{변수 이름}



React.js UI에서 바뀐 부분만 업데이트를 해준다.

이는 엄청나게 중요한 부분이다.

React.js는 이전에 렌더링된 컴포넌트는 어떤 것인지 확인한다.

그리고 다음에 렌더링될 컴포넌트는 어떤 것인지 확인한다.

그리고 둘 사이에 차이가 있는 부분만 업데이트를 해준다.

이는 인터렉티브한 애플리케이션을 만들 수 있다는 점에서 굉장히 좋다.



리렌더링을 하는 좀 더 좋은 방법

리액트JS에서 데이터를 저장시켜 자동으로 리렌더링을 일으킬 수 있는 방법
const data = React.useState();를 console.log 시키면
[undefined, f ] -> undefined와 함수가 적힌 배열이 나타남
undefined는 data이고 f는 data를 바꿀 때 사용하는 함수
React.useState() 함수는 초기값을 설정할 수 있음
즉, undefined는 초기값이고 두 번째 요소인 f는 그 값을 바꾸는 함수

배열을 꺼내기
const x = [1, 2, 3]
const [a, b, c] = x;
으로 꺼낼 수 있음

```jsx
<script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
    const root = document.getElementById("root");
    function App() {
        const [counter, setCounter] = React.useState(0);
        const onClick = () => {
            setCounter(counter+1);

        };
        return (
        <div>
        <h2>Total clicks : {counter}</h2>
        <button onClick={onClick}>Click Me!</button>
        </div>)
    };
    ReactDOM.render(<App />, root)
</script>
```



React.useState() 배열에서
보통 데이터에는 counter처럼 원하는대로 붙이고
f는 set 뒤에 데이터 이름을 붙임 (setCounter)
어떤값을 부여하던 setCounter 함수는 그 값으로 업데이트하고 리렌더링 일으킴
\1. counter라는 데이터를 받음
\2. return()에 그 데이터를 담고 있음 (리턴은 사용자가 보게될 컴포넌트)
\3. 버튼이 클릭되면 counter값을 바꿔줄 함수 호출 -> setCounter
\4. counter의 새로운 값을 가지고 counter 함수를 호출
\5. 그 새로운 값은 setCounter(counter + 1)에 써준 counter + 1

const [counter, setCounter] = React.useState(); 에서
React.useState() 는 react기능을 쓸 수있게 해주는 하나의 도구이고,
counter은 현재의 값 state 이며,
setCounter은 이벤트 발생시 변화를 주는 부분이어서 이후 counter로 다시 저장된다.

React.js는 똑똑한 기능을 가지고 있기 때문에 매번 자동으로 바뀌는 리렌더링해준다.
하지만! 그냥 똑똑한게 아니라 엄청 똑똑하기 때문에 '실제로 바뀌는 값'만 판단해서 불필요한 리렌더링을 제외한 채로 동작한다!!!



state가 변경되면 리렌더링이 일어난다.

state를 바꾸는 2가지 방법
\1. setCounter를 이용해 원하는 값을 넣어주기
\2. 함수를 이용해서 현재 값을 계산하기
setCounter((current) => current + 1);
-> 함수가 뭘 return하던지 그것이 새로운 state가 됨
-> setCounter(counter + 1)와 똑같은 기능이지만 더 안전함



JSX는 class 혹은 for 과 같이 JavaScript에서 이미 선점된 문법 용어를 사용하지 못한다.
그래서 class는 className으로 for은 htmlFor로 바꿔야한다.



react, reactdom을 import하는 script tag에서
production - > development 로 변경하는데
production은 배포 모드, development는 개발 모드를 의미합니다.
개발모드는 니코쌤이 보여준 것 처럼 버그로 이어질 수 있는 요소들을 미리 경고하는 검증 코드가 포함되어 있습니다.



useState는 array를 제공함. 그 첫 번째 element가 현재의 값
사용자가 input에 다른 값을 입력할 때마다 value를 업데이트 시키는 것
사용자가 input에 뭔가를 입력하면 onChange라는 이벤트를 리스닝
-> input에 변화가 생기면 onChange 함수 실행됨
-> 그래서 우리가 입력한 input의 value를 바탕으로 component를 업데이트 해줌



flip
const onFlip = () => setFlipped(!flipped);
-> flipped가 true라면 부정명제인 !flipped는 false
-> false라면 true

state값으로 input을 enabled할지 disabled 할지를 결정할 수 있음

디폴트 값이 false 라고 정했으므로 Hours는 disabled 되어야함
그래서 disabled={flipped === false}를 써줘서
flipped가 false라면, disabled는 true가 되도록 만들어줌
Minuets에는 반대로
disabled={flipped === true}라고 써줌
그러나
Hours는
disabled={!flipped}
Minuets에는 반대로
disabled={flipped}
주는게 더 짧고 좋음

시간 -> 분 컨버터
삼항연산자(ternary operator) 사용하기
flipped ? amount : amount / 60
-> 만약 flipped 상태면 state에 있는 값을 그대로 보여주기
아니라면 60으로 나눈 변환된 값 보여주기
value={flipped ? amount * 60 : amount}
-> 만약 flipped 상태면 60으로 곱한 변환된 값 보여주기
아니라면 state에 있는 값을 그대로 보여주기

flip누르면 변화된 값 그대로 가져오므로
onFlip 변수에 reset(); 넣어주기



reactjs는 자동으로 내가 넣는 모든 props을 

object안으로 집어넣고,

이 object는 컴포넌트의 첫 번째 인자가 된다. 

props는 첫번째이자 유일한 인자이다. 

