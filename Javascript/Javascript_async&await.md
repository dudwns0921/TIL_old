# 비동기 처리와 async & await

async와 await는 자바스크립트의 비동기 처리 패턴 중 가장 최근에 나온 문법이다. 기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성할 수 있게 도와준다.

## async & await 기본 문법

```js
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```

먼저 함수의 앞에 `async` 라는 예약어를 붙인다. 그러고 나서 함수의 내부 로직 중 HTTP 통신을 하는 비동기 처리 코드 앞에 `await`를 붙인다. 여기서 주의해야 할 점은 비동기 처리 메서드가 꼭 프로미스 객체를 반환해야 `await`가 의도한 대로 동작한다.

## Promise와 비교해보기

```javascript
const getMoviesPromise = () => {
fetch("https://yts.mx/api/v2/list_movies.json")
.then((response) => response.json();)
.then((movies) => console.log(movies))
.catch((e) => console.log(`✔${e}`));
};

const getMoviesAsync = async () => {
const response = await fetch("https://yts.mx/api/v2/list_movies.json");
const json = await response.json();
console.log(json);
};

getMoviesAsync();
```

Promise에서 반환된 값을 가공하기 위해서는 여러 개의 then을 사용하는 수밖에 없다. 이 때문에 가독성이 떨어지는 콜백 지옥의 문제점이 다시금 나타나게 된다.

async & await에서는 반환된 값을 변수에 할당할 수 있어 좀 더 직관적이고 보기 좋은 코드를 쓸 수 있게끔 했다. 굳이 설명을 덧붙이지 않아도 두 개를 비교하면 어떤 것이 더 깔끔한지 확연하게 알 수 있을 것이다.

## async & await 예외 처리

async & await에서 예외를 처리하는 방법은 바로 try catch문을 활용하는 것이다. 프로미스에서 에러 처리를 위해 `.catch()`를 사용했던 것처럼 async에서는 `catch {}` 를 사용하시면 된다.

조금 전 코드에 바로 `try catch` 문법을 적용해보겠다.

```javascript
const getMoviesAsync = async () => {
  try {
    const response = await fetch('https://yts.mx/api/v2/list_movies.json');
    const json = await response.json();
    console.log(json);
  } catch (e) {
    console.log(e);
  }
};

getMoviesAsync();
```

위의 코드를 실행하다가 발생한 네트워크 통신 오류뿐만 아니라 간단한 타입 오류 등의 일반적인 오류까지도 `catch`로 잡아낼 수 있다. 발견된 에러는 `error` 객체에 담기기 때문에 에러의 유형에 맞게 에러 코드를 처리해주시면 된다.

# :books:참고자료

https://joshua1988.github.io/web-development/javascript/js-async-await/

노마드코더 강의
