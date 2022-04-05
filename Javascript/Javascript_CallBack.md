# 비동기 처리와 콜백함수

## 비동기 처리란?

자바스크립트의 비동기 처리란 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 자바스크립트의 특성을 의미한다.

이러한 특성 때문에 비동기 함수는 자바스크립트처럼 싱글 쓰레드 환경에서 실행되는 언어에서 광범위하게 사용된다. 예를 들어, 브라우저에서 어떤 로직이 동기 함수만으로 실행될 경우, 기다리는 시간이 많아져서 사용자 경험에 부정적인 영향을 미칠 것이다. 또한, 비동기 함수를 사용하면 로직을 순차적으로 처리할 필요가 없기 때문에 동시 처리에서도 동기 함수 대비 유리한 것으로 알려져 있다. 비동기 처리는 이처럼 효율성을 올려주지만 동시에 부작용도 존재한다.

## 비동기 처리의 문제

비동기 처리의 가장 흔한 사례인 제이쿼리의 ajax 예시를 통해 비동기 처리의 문제점에 대해서 알아보자. 

```js
function getData() {
    var tableData;
    $.get('https://domain.com/products/1', function(response) {
        tableData = response;
    });
    return tableData;
}

console.log(getData()); // undefined
```

전체적인 과정을 정리해보자면, 

1. `https://domain.com/products/1` 에 get 요청을 보내 1번 상품의 데이터를 받아온다.

2. 해당 데이터를 tableData 변수에 할당한다.

3. 콘솔에 tableData 변수를 출력한다.

비동기 처리에 대한 이해가 없다면 tableData에는 1번 상품의 데이터가 담겨있다고 생각할 수도 있다. 하지만 실제로는 undefined가 출력된다. 그 이유는 `$.get()`로 데이터를 요청하고 받아올 때까지 기다려주지 않고 다음 코드인 `return tableData;`를 실행했기 때문이다. 따라서, getData()의 결과 값은 초기 값을 설정하지 않은 tableData의 값 undefined를 출력한다.

이처럼 비동기 처리는 순차적 처리가 보장되지 않기 때문에 코드 실행 순서가 뒤죽박죽될 수 있다. 

## 콜백 함수로 비동기 처리 방식의 문제점 해결하기

이러한 문제는 콜백 함수를 이용해 해결할 수 있다. 

```js
function getData(callbackFunc) {
    $.get('https://domain.com/products/1', function(response) {
        callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
    });
}

getData(function(tableData) {
    console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

이렇게 콜백 함수를 사용하면 특정 로직이 끝났을 때 원하는 동작을 실행시킬 수 있다.

## 콜백 지옥 (Callback hell)

콜백 지옥은 비동기 처리 로직을 위해 콜백 함수를 연속해서 사용할 때 발생하는 문제이다.

```js
$.get('url', function(response) {
    parseValue(response, function(id) {
        auth(id, function(result) {
            display(result, function(text) {
                console.log(text);
            });
        });
    });
});
```

웹 서비스를 개발하다 보면 서버에서 데이터를 받아와 화면에 표시하기까지 인코딩, 사용자 인증 등을 처리해야 하는 경우가 있다. 만약 이 모든 과정을 비동기로 처리해야 한다고 하면 위와 같이 콜백 안에 콜백을 계속 무는 형식으로 코딩을 하게 된다.

왜냐하면 콜백함수로 반환한 값을 사용하기 위해서는 해당 콜백 함수 내에서 새로운 콜백 함수를 호출해서 사용해야 하기 때문입니다. 이러한 코드 구조는 가독성도 떨어지고 로직을 변경하기도 어렵습니다. 이와 같은 코드 구조를 콜백 지옥이라고 한다.

# :books: 참고자료

노마드코더 강의

https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/

[https://www.daleseo.com/js-async-callback/](https://www.daleseo.com/js-async-callback/)
