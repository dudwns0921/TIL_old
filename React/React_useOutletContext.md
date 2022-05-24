# React

## 전역 state의 편리함

회사에서는 프로젝트를 진행할 때 Vue를 사용한다. 회사에서 GUI 관련 업무를 하면서 나도 모르게 내가 Vuex에 많이 의존하고 있다는 사실을 알게 되었다. 컴포넌트들 사이의 복잡한 관계를 분석할 필요없이 전역 state를 사용함으로써 간단히 해결되는 문제들이 많았기 때문이다. 그래서 사이드 프로젝트에서라도 state 관리 라이브러리를 쓰지 않고 문제를 해결해보고 싶었다.

내가 하고 만들고 있는 사이드 프로젝트는 React + express 의 형태이며, 내가 하려고 했던 일은 로그인 상태에 따라 네비게이션 바의 모습을 바꾸는 것이었다. 만약 Redux를 사용한다면 정말 간단하게 처리할 수 있는 문제였다. 하지만 Redux를 쓰지 않으려고 하니 꽤나 어려운 문제처럼 느껴졌다.

### 프로젝트 구조 분석

```react
// App.js

const Content = styled.div`
  display: flex;
  flex-direction: column;
...
`
function App() {
...
  return (
    <div>
      <NavBar />
      <Content>
        <Outlet />
      </Content>
    </div>
  )
}
```

```react
// index.js

<Routes>
    <Route path="/" element={<App />}>
        <Route index element={<Home />} />
        <Route path="join" element={<Join />} />
        <Route path="login" element={<Login />} />
        <Route path="upload" element={<Upload />} />
        <Route path="video/:id" element={<Video />} />
        <Route path="videoEdit/:id" element={<VideoEdit />} />
        <Route path="search/:keyword" element={<Search />} />
        <Route path="mypage" element={<MyPage />} />
        <Route path="userEdit" element={<UserEdit />} />
        <Route path="githubLoginProcess" element={<GIthubLoginProcess />} />
        <Route path="*" element={<NotFound />} />
    </Route>
</Routes>
```

위에서부터 각각 루트 컴포넌트인 App.js와 라우터가 정의되어있는 index.js 이다. 

#### Nested Routes

index.js 파일을 보면 App Route 컴포넌트가 다른 여러 Route 컴포넌트들을 포함하고 있는 걸  확인할 수 있다. 이는 Nested Routes를 사용한 것으로, 여러 자식 Route 컴포넌트들이 부모 Route 컴포넌트인 App 컴포넌트의 url를 상속한다. 

- '/' + 'login' = '/login' -> login 컴포넌트 렌더링

컴포넌트 트리의 모습은 다음과 같다.

```react
<App>
	<Login />
</App>
```

자식 Route 컴포넌트들은 부모 컴포넌트의 Outlet 컴포넌트에 렌더링된다. 이에 따라 좀 더 구체적으로 살펴보자면 아래와 같다. 

```react
<div>
	<NavBar />
	<Content>
		<Login />
	</Content>
</div>
```

여기까지 글을 읽었다면 약간 의아함이 들 수도 있다. 네비게이션 바의 모습을 바꾸는데 왜 router 구조를 살펴보고 있는지 말이다. 이렇게 route 구조를 살펴본 이유는 루트 컴포넌트인 App 컴포넌트에서 state를 자식 컴포넌트한테 쉽게 전달할 수 있는 방법이 있다면, 굳이 redux를 쓰지 않아도 되기 때문이다. 

## useOutletContext

useOutletContext는 Outlet의 내장 함수로 Outlet 컴포넌트의 context의 prop에 배열로 넘긴 데이터, 함수 등을 반환한다. 주의할 점은 prop으로 넘겨줄 때나 반환받을 때나 항상 배열의 형태를 사용해야 한다는 점이다.

```react
// App.js

function App() {
  const [isLogin, setIsLogin] = useState(false)
...
  return (
    <div>
      <NavBar isLogin={isLogin} />
      <Content>
        <Outlet context={[setIsLogin]} />
      </Content>
    </div>
  )
}
```

```react
// Login.js

function Login() {
  const [setIsLogin] = useOutletContext()
  // Redux 대신 props로 state와 setState를 전달
  ...
const handleSubmit = async (e) => {
  e.preventDefault()
  const { data } = await loginUser(userData)
  if (data.result === 'success') {
    saveTokenToCookie(data.token)
    saveUserToCookie(JSON.stringify(data.user))
    if (getTokenFromCookie() && getUserFromCookie()) {
      setIsLogin(true)
      navigate('/')
    }
  }
}
```

위 로직은 로그인이 성공하면 백엔드에서 받아온 정보들을 쿠키에 저장한 다음에 useOutletContext에서 받아온 setIsLogin 함수로 로그인 상태를 갱신시키고 있다. 다행히 잘 작동한다!

## 마무리

### Redux의 3가지 원칙

- #### Single source of truth

- #### State is read-only

- #### Changes are made with pure functions

이번에 prop을 통해 state를 관리했음에도, Redux의 3가지 원칙이 자연스럽게 지켜지는 걸 알 수 있었다.

첫 번째 원칙은 state들은 하나의 공간에 저장되어야 한다는 의미인데, 이번에는 전역에서 사용한 state가 하나라서 큰 의미는 없다. 그래도 state가 더 추가된다면 계속 루트 컴포넌트인 App에 추가할 것이기 때문에 해당 원칙을 지킨다고 볼 수 있다.

두 번째 원칙은 state는 읽기 전용이기에 불변성을 지켜야 하며 직접적인 변경이 일어나서는 안된다는 의미이다. 다음은 리액트 공식 문서에서 발췌한 내용이다.

> props는 읽기 전용입니다.
>
> 함수 컴포넌트나 클래스 컴포넌트 모두 컴포넌트의 자체 props를 수정해서는 안 됩니다.

props도 동일한 특징을 지닌다는 걸 확인할 수 있다.

마지막 원칙은 state에 대한 변경은 순수 함수로만 이루어져야 한다는 것인데, 여기서 순수함수란 동일한 인자가 주어졌을 때 항상 동일한 결과를 반환해야 하며, 외부의 상태를 변경하지 않는 함수를 의미한다. 즉, 함수 내 변수 외에 외부의 값을 참조, 의존하거나 변경하지 않는 함수이다. 

> 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 합니다.

마지막 원칙도 prop을 사용할 때 지켜야 하는 원칙과 동일하다.

여기서 느낀 점은 Redux가 없어도 컴포넌트 간의 관계를 잘 구성하고 파악만 한다면 충분히 state 관리를 할 수 있다는 점이다. 물론 실제 서비스와 같이 정말 수많은 state들이 사용되어 관리가 어려울 때는 Redux를 사용하는 게 좋겠지만 토이 프로젝트 정도의 규모에서 과연 Redux와 같은 state 관리 라이브러리를 사용해야 하나 라는 생각이 든다.

# :books:참고자료

https://reactrouter.com/docs/en/v6/hooks/use-outlet-context (2022.05.24 기준 해당 내용이 없어졌다.)

https://velog.io/@daeseongkim/Redux%EC%9D%98-3%EA%B0%80%EC%A7%80-%EC%9B%90%EC%B9%99
