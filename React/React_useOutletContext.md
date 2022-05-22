# React

## 전역 state의 편리함

회사에서는 프로젝트를 진행할 때 Vue를 사용한다. 회사에서 GUI 관련 업무를 하면서 나도 모르게 내가 Vuex에 많이 의존하고 있다는 사실을 알게 되었다. 컴포넌트들 사이의 복잡한 관계를 분석할 필요없이 전역 state를 사용함으로써 간단히 해결되는 문제들이 많았기 때문이다. 그래서 사이드 프로젝트에서라도 상태관리 라이브러리를 쓰지 않고 문제를 해결해보고 싶었다.

내가 하려고 했던 일은 로그인 상태에 따라 네비게이션 바의 모습을 바꾸는 것이었다. 만약 redux를 사용한다면 정말 간단하게 처리할 수 있는 문제였다. 하지만 redux를 쓰지 않으려고 하니 꽤나 어려운 문제처럼 느껴졌다.

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

- '/' + 'join' = '/join' -> Join 컴포넌트 렌더링

컴포넌트 트리의 모습은 다음과 같다.

```react
<App>
	<Join />
</App>
```

자식 Route 컴포넌트들은 부모 컴포넌트의 Outlet 컴포넌트에 렌더링된다. 이에 따라 좀 더 구체적으로 살펴보자면 아래와 같다. 

```react
<div>
	<NavBar />
	<Content>
		<Join />
	</Content>
</div>
```

여기까지 글을 읽었다면 약간 의아함이 들 수도 있다. 네비게이션 바의 모습을 바꾸는데 왜 router 구조를 살펴보고 있는지 말이다. 이렇게 route 구조를 살펴본 이유는 루트 컴포넌트인 App 컴포넌트에서 state를 자식 컴포넌트한테 쉽게 전달할 수 있는 방법이 있다면, 굳이 redux를 쓰지 않아도 되기 때문이다. 

## useOutletContext

useOutletContext는 Outlet의 내장 함수로 Outlet 컴포넌트의 context의 prop에 배열로 넘긴 데이터, 함수 등을 반환한다.

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
```









