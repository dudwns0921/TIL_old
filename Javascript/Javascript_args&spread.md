# 모던 자바스크립트

## 챕터 : 나머지 매개변수와 스프레드 문법

### 나머지 매개변수 ...

함수 정의와 상관없이 함수에 넘겨주는 인수의 개수에는 제약이 없다.

```js
function sum(a, b) {
  return a + b;
}

console.log(sum(1, 2, 3, 4, 5, 6, 7)); // 3
```

함수를 정의할 땐 인수를 두 개만 받도록 하고, 실제 함수를 호출할 땐 이보다 더 많은 ‘여분의’ 인수를 전달했지만, 에러가 발생하지 않았습니다. 다만 반환 값은 처음 두 개의 인수만을 사용해 계산된다.

이렇게 여분의 매개변수는 그 값들을 담을 배열 이름 뒤에 마침표 세 개 `...`를 붙여주면 함수 선언부에 포함시킬 수 있다. 이때 마침표 세 개 `...`는 "남아있는 매개변수들을 한데 모아 배열에 집어넣어라."는 것을 의미한다.

아래 예시에서는 모든 인수가 배열 args에 모인다.

```js
function sumAll(...args) {
  // args는 배열의 이름입니다.
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}

console.log(sumAll(1)); // 1
console.log(sumAll(1, 2)); // 3
console.log(sumAll(1, 2, 3)); // 6
```

앞부분의 매개변수는 변수로, 남아있는 매개변수들은 배열로 모을 수도 있다.

```js
function whoYouAre(firstName, lastName, ...skillStacks) {
  console.log(`My name is ${firstName} ${lastName}.`);
  let skillStacksString = skillStacks.join(', ');
  console.log(`And I'm good at ${skillStacksString}!`);
}

whoYouAre('Young Jun', 'Jung', 'Vue', 'React', 'Python');  

// My name is Young Jun Jung.
// And I'm good at Vue, React, Java!
```

**나머지 매개변수는 위의 예시처럼 항상 마지막에 있어야 한다는 점을 주의하자.**

## :bulb:Tip

이전에는 함수의 인수 전체를 얻어내기 위해 유사 배열 객체인 arguments를 사용했다. 지금도 사용가능하지만, 다음과 같은 단점이 있다.

- 배열 메서드 사용 불가

- 인수 전체를 담기 때문에 인수의 일부만 사용할 수 없음

- 화살표 함수에서 사용불가

### 스프레드 문법

지금까지 매개변수 목록을 배열로 가져오는 방법에 대해 살펴보았다. 그런데 개발을 하다 보면 배열을 통째로 매개변수에 넘겨야 하는 경우가 종종 발생하고는 한다.

Math.max 함수를 예시로 살펴보겠다.

```js
console.log(Math.max(3,5,1)); // 5
```

만약 배열 `[3, 5, 1]`이 있고, 이 배열을 대상으로 `Math.max`를 호출하고 싶다고 가정해보겠다. 아무런 조작 없이 배열을 있는 그대로 `Math.max`에 넘기면 원하는 대로 동작하지 않는다. 왜냐하면 `Math.max`는 배열이 아닌 숫자 목록을 인수로 받기 때문이다.

스프레드 문법은 이러한 상황을 위해 만들어졌다. `...`을 사용하기 때문에 나머지 매개변수와 비슷해 보이지만, 스프레드 문법은 앞에서 말했다시피 나머지 매개변수와 반대되는 역할을 한다.

함수를 호출할 때 `...arr`를 사용하면, 이터러블 객체 `arr`이 인수 목록으로 확장된다.

```js
let arr = [3, 5, 1];

console.log( Math.max(...arr) ); // 5 
```

아래와 같이 이터러블 객체 여러 개를 전달하는 것도 가능하다.

```js
let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

console.log( Math.max(...arr1, ...arr2) ); // 8
```

스프레드 문법을 평범한 값과 혼합해 사용하는 것도 가능하다.

```js
let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

console.log( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25
```

스프레드 문법은 배열을 합칠 때도 활용할 수 있다.

```js
let arr = [3, 5, 1];
let arr2 = [8, 9, 15];

let merged = [0, ...arr, 2, ...arr2];

console.log(merged); // 0,3,5,1,2,8,9,15 (0, arr, 2, arr2 순서로 합쳐진다.)
```

지금까지 배열을 대상으로 스프레드 문법이 어떻게 동작하는지 살펴봤다. 그런데 배열이 아니더라도 이터러블 객체이면 스프레드 문법을 사용할 수 있다.

```js
let str = 'Hello';
console.log([...str]); // [ 'H', 'e', 'l', 'l', 'o' ]
```

스프레드 문법은 `for..of`와 같은 방식으로 내부에서 이터레이터를 사용해 요소를 수집한다. 문자열에 `for..of`를 사용하면 문자열을 구성하는 문자가 반환된다.

메서드 `Array.from`은 이터러블 객체인 문자열을 배열로 바꿔주기 때문에 `Array.from`을 사용해도 동일한 작업을 할 수 있다.

하지만 `Array.from`은 이터러블 객체에만 사용 가능한 스프레드 문법과 달리 유사 배열 객체와 이터러블 객체 둘 다에 사용할 수 있기 때문에 무언가를 배열로 바꿀 때는 일반적으로 `Array.from`이 사용된다.

### 배열과 객체의 복사본 만들기

```js
let arr = [1, 2, 3];
let arrCopy = [...arr];
// 배열을 펼쳐서 각 요소를 분리후, 매개변수 목록으로 만든 다음에
// 매개변수 목록을 새로운 배열에 할당함

// 배열 복사본의 요소가 기존 배열 요소와 진짜 같은지 확인
alert(JSON.stringify(arr) === JSON.stringify(arrCopy)); // true

// 두 배열은 같은지 확인
alert(arr === arrCopy); // false (참조가 다름)

// 참조가 다르므로 기존 배열을 수정해도 복사본은 영향을 받지 않는다.
arr.push(4);
alert(arr); // 1, 2, 3, 4
alert(arrCopy); // 1, 2, 3
```

## :man_technologist:Extra Study

### JSON란?

JavaScript Object Notation (JSON)은 Javascript 객체 문법으로 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷이다. 웹 어플리케이션에서 데이터를 전송할 때 일반적으로 사용한다.

자바스크립트에는 JSON 내장 객체가 존재하는데, JSON을 분석하거나 값을 JSON으로 변환하는 메서드를 가지고 있다. 그 중 **`JSON.stringify()`** 는 JavaScript 값이나 객체를 JSON 문자열로 변환한다.

### 동등 연산자와 일치 연산자의 차이

동등 연산자는 일치 연산자와 다르게 다른 타입의 피연산자들끼리의 비교를 시도한다.

```js
console.log('1' ==  1);
// true
```

___

```js
let obj = { a: 1, b: 2, c: 3 };
let objCopy = { ...obj };
// 객체를 펼쳐서 각 요소를 분리후, 매개변수 목록으로 만든 다음에
// 매개변수 목록을 새로운 객체에 할당함

// 객체 복사본의 프로퍼티들이 기존 객체의 프로퍼티들과 진짜 같은지 확인
alert(JSON.stringify(obj) === JSON.stringify(objCopy)); // true

// 두 객체는 같은지 확인
alert(obj === objCopy); // false (참조가 다름)

// 참조가 다르므로 기존 객체를 수정해도 복사본은 영향을 받지 않는다.
obj.d = 4;
alert(JSON.stringify(obj)); // {"a":1,"b":2,"c":3,"d":4}
alert(JSON.stringify(objCopy)); // {"a":1,"b":2,"c":3}
```

# :books:참고자료

https://ko.javascript.info/rest-parameters-spread


