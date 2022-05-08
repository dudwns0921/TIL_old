# 비동기 처리와 Promise

이전에 콜백함수의 문제인 콜백 지옥에 대해 알아본 적이 있다. 콜백 지옥을 해결하기 위해서는 Promise나 Async 함수를 사용하면 되는데, 먼저 Promise에 대해 알아보자.

[비동기 처리와 콜백함수](./Javascript_CallBack.md)

## Promise

프로미스는 자바스크립트 비동기 처리에 사용되는 객체이다. 프로미스의 핵심은 아직 정의되지 않은 데이터들로 작업을 할 수 있다는 점이다. 

## 프로미스의 3가지 상태(states)

프로미스를 사용할 때 알아야 하는 가장 기본적인 개념이 바로 프로미스의 상태(states)이다. 여기서 말하는 상태란 프로미스의 처리 과정을 의미한다. `new Promise()`로 프로미스를 생성하고 종료될 때까지 3가지 상태를 갖는다.

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

#### Pending(대기)

먼저 아래와 같이 `new Promise()` 메서드를 호출하면 대기(Pending) 상태가 된다.

```js
new Promise();
```

`new Promise()` 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 `resolve`, `reject`이다.

```js
new Promise(function(resolve, reject) {
  // ...
});
```

#### Fulfilled(이행)

여기서 콜백 함수의 인자 `resolve`를 아래와 같이 실행하면 이행(Fulfilled) 상태가 된다.

```js
new Promise(function(resolve, reject) {
  resolve();
});
```

그리고 이행 상태가 되면 아래와 같이 `then()`을 이용하여 처리 결과 값을 받을 수 있다.

```js
function getData() {
  return new Promise(function(resolve, reject) {
    var data = 100;
    resolve(data);
  });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function(resolvedData) {
  console.log(resolvedData); // 100
});
```

※ 프로미스의 '이행' 상태를 좀 다르게 표현해보면 '완료' 이다.

#### Rejected(실패)

`new Promise()`로 프로미스 객체를 생성하면 콜백 함수 인자로 `resolve`와 `reject`를 사용할 수 있다고 했는데, 여기서 `reject`를 아래와 같이 호출하면 실패(Rejected) 상태가 된다.

```js
new Promise(function(resolve, reject) {
  reject();
});
```

그리고, 실패 상태가 되면 실패한 이유(실패 처리의 결과 값)를 `catch()`로 받을 수 있다.

```js
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData().then().catch(function(err) {
  console.log(err); // Error: Request is failed
});
```

## 프로미스 체이닝

프로미스의 또 다른 특징은 여러 개의 프로미스를 연결하여 사용할 수 있다는 점이다. 앞 예제에서 `then()` 메서드를 호출하고 나면 새로운 프로미스 객체가 반환된다. 따라서, 아래와 같이 코딩이 가능하다.

```javascript
const amIdouble = new Promise((resolve, reject) => {
  resolve(2);
});

const timesTwo = numbertwo => numbertwo * 2;

amIdouble
  .then(timesTwo)
  .then(timesTwo)
  .then(timesTwo)
  .then(timesTwo)
  .then(lastNumber => console.log(lastNumber))
```

반환된 프로미스 객체의 값에 계속 2를 곱해주는 코드이고, 결과적으로 2의 5제곱인 32를 반환하게 된다. 

## 프로미스 에러 처리 방법

```javascript
amIdouble
.then(timesTwo)
.then(timesTwo)
.then(timesTwo)
.then(timesTwo)
.then(() => {
throw Error("something is wrong");
})
.then((lastNumber) => console.log(lastNumber))
.catch((error) => console.log(error));
```

프로미스의 에러를 처리하기 위해서는 catch 구문을 이용하면 된다.

위의 예제를 catch 구문을 더해 약간 수정해보았다. 중간에 일부러 에러를 발생시켜 해당 에러 메시지를 콘솔에 출력했다. 코드만 봤을 때는 마치 순차적으로 진행되고 마지막에 catch가 실행되는 것처럼 보인다. 하지만 프로미스에서 어떠한 상태를 반환하는지에 따라 then이 실행되거나 catch가 실행되는 것이지 순차적으로 진행되는 것이 아니다.

```javascript
amIdouble
  .then(timesTwo)
  .catch(error => console.log(error))
  .then(() => {
    throw Error('something is wrong');
  })
  .then(timesTwo)
  .then(timesTwo)
  .then(lastNumber => console.log(lastNumber));
```

코드를 이런 식으로 구성해도 콘솔창에는 에러 메시지가 그대로 출력되는 걸 확인할 수 있다. 

# :books:참고자료

[Promise - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

https://joshua1988.github.io/web-development/javascript/promise-for-beginners/
