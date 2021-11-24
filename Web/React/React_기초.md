# React 기초

## React를 왜 사용해야 할까?

### 1. 엄청난 사용량

먼저 전 세계의 상위 1만 개의 웹 사이트 중 44.76%는 ReactJS를 사용한다. (2021년 7월 기준)



### 2. 세계 주요 기업들이 사용

ReactJS를 사용하는 회사는 메타, 에어비앤비, 넷플릭스 등으로 굉장히 세계적인 기업들이다.

이들에게 웹사이트는 매우 중요한 요소일텐데 그들이 ReactJS를 사용한다는 것은 ReactJS가 그만큼 안정적이라는 걸 보여준다.

그리고 ReactJS를 만든 메타는 여전히 ReactJS를 개선시키려고 엄청난 투자를 하고 있다.



### 3. 거대한 커뮤니티

ReactJS는 거대한 커뮤니티를 가지고 있다.

ReactJS는 Javascript와 거의 동일해서 Javascript의 커뮤니티가 ReactJS의 커뮤니티라고 봐도 무방하다.

그렇기 때문에 커뮤니티의 규모가 엄청나다.

커뮤니티의 규모가 큰 만큼 책이나 수업, 가르쳐줄 사람, 채용 시장 등의 규모 또한 정말 크다.



## React 시작하기

### 1단계: HTML 파일에 DOM 컨테이너 설치

비어있는 html 파일의 body에 빈 div 태그를 추가한다.

이 태그가 바로 React를 통해 원하는 내용을 표시할 수 있는 위치가 된다.

```html
<body>
    <div id="root"></div>
</body>
```



### 2단계: 스크립트 태그 추가하기

```html
<script src="https://unpkg.com/react@17.0.2/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.development.js" crossorigin></script>
```



### 3단계: React 컴포넌트 만들기

```html
<script>
    const root = document.getElementById("root");
    const Title = Re
        <h3
        onMouseEnter={()=>console.log("You entered title")}
        >I'm Title</h3>
    );
    const btn = React.createElement("button", {
        id: "sexybtn", 
    	onClick: ()=>console.log("Hi"),
        style: {
            backgroundColor: "tomato";
        }
    }, "Click me");
    ReactDOM.render(btn, root);
</script>
```

HTML 파일에 추가한 div 태그를 찾아주고 그 안에 btn이라는 React 컴포넌트를 추가하는 과정이다.



React.createElement("태그명", {속성 정의}, "태그 안의 값")

위의 예시에서는 id의 값을 sexybtn으로 정의하고, 배경색을 tomato로 설정한 후 클릭했을 때 콘솔창에 Hi 라는 문자열이 출력되도록 했다.



ReactDOM.render(추가하려는 React 컴포넌트, 그 컴포넌트를 생성하려는 위치)

위의 예시에서는 btn이라는 React 컴포넌트를 id가 root라는 div에 추가했다. 



## JSX

JSX 자바스크립트를 확장한 문법으로 HTML과 상당히 유사하다.

그렇기 때문에 개발자들 입장에서는 JSX로 React 요소를 만드는 것이 위에서 살펴본 방법보다 편하다.

```jsx
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
    const root = document.getElementById("root");
    const Title = () => (
        <h3
        onMouseEnter={()=>console.log("You entered title")}
        >I'm Title</h3>
    );
    const Button = () => (
        <button
        style={{backgroundColor: "red",}}
        onClick={() => console.log("Hi!")}
        >Click Me!</button>
    );
    const Container = <div>
        <Title />
        <Button />
        </div>
    ReactDOM.render(Container, root);
</script>
```

위의 예시를 JSX로 바꿔보았다.

주의해야 할 점은 브라우저가 JSX를 완벽하게 이해하는 것이 아니기 때문에 babel이라는 컴파일러를 통한 변환 작업이 필요하다.







# :books:참고자료

https://ko.reactjs.org/docs/add-react-to-a-website.html

노마드코더 ReactJS로 영화 웹 서비스 만들기 강의 참고