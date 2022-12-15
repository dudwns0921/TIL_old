# React

## react-router-dom

아래 작성된 내용은 react-router-dom 6.4.5 버전을 토대로 작성되었다.

### useNavigate

useNavigate hook은 페이지 이동을 할 수 있는 함수를 반환한다. 

####  Type Declaration

```ts
declare function useNavigate(): NavigateFunction;

interface NavigateFunction {
  (
    to: To,
    options?: {
      replace?: boolean;
      state?: any;
      relative?: RelativeRoutingType;
    }
  ): void;
  (delta: number): void;
}
```

Type Declaration을 확인해보면 페이지 이동을 두 가지 방법으로 할 수 있다. Link 컴포넌트의 to 프로퍼티에 url을 전달하듯이 문자열 형식의 url을 전달하거나 delta 라는 숫자를 전달하면 된다.

이 때 delta는 history stack의 인덱스를 의미하는데, 가령 -1일경우 뒤로 가기 버튼을 누른 것과 동일하다. 같은 원리로 0을 전달하면 메인 페이지로 새로고침이 되는데, history stack에 쌓인 첫 번째 history로 이동하기 때문이다.

# :books:참고자료

https://reactrouter.com/en/main





