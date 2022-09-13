# Javascript

## 깊은 복사

먼저 얕은 복사와 깊은 복사에 대해서 알아보자.

- 얕은 복사 : 객체의 참조값을 복사
- 깊은 복사 : 객체의 실제값을 복사

자바스크립트에서 깊은 복사를 하는 방법에는 2가지가 있다. 먼저 가장 흔하게 쓰이는 방법부터 살펴보자.

## JSON.parse/stringify

회사 선임분이 알려주신 코드로 실무에서도 널리 쓰이는 코드이다.

```js
const defaultObj = { a : 'b' };
const deepCopy = JSON.parse(JSON.stringify(defaultObj));

console.log( params.a ); // b
console.log( defaultParams.a ); // b
console.log( params === defaultParams ); // false
```

하지만 이 방법에는 한 가지 단점이 있다.

#### 데이터 손실

위 방법으로 간단한 원시 값을 깊은 복사하는데는 큰 문제가 없다. 하지만 아래의 값을 다루면 데이터 손실이 발생할 수 있다.

- `Date`
- `function`
- `undefined`
- `Infinity`
- `RegExp`
- `Map`
- `Set`
- `Blob`
- `FileList`
- `ImageData`
- `sparse Array`
- `Typed Array` 등

예시를 통해 살펴보자.

```js
const obj = {
  date: new Date(),  // stringified
  undef: undefined,  // lost
  inf: Infinity,  // forced to 'null'
  re: /.*/,  // lost
}

const deepCopyObj = JSON.parse(JSON.stringify(a))

console.log(deepCopyObj)

// {
//   date: '2022-08-31T00:07:21.144Z', 
//   inf: null, 
//   re: {}
// }
```

# structuredClone()

HTML 표준에 의해 정의된 새로운 알고리즘인 `structured clone algorithm`을 활용한 전역 메서드로 주어진 값의 깊은 복사본을 만든다. 낮은 `node` 버전에서는 

> ReferenceError: structuredClone is not defined

위 에러가 발생할 수 있으니 가능하면 최신 버전의 `node` 를 사용하자. 

앞에서 살펴본 방법과 달리 아래의 값도 복사가 가능하다.

- `RegExp`
- `Blob`
- `File`
- `FileList`
- `ImageData` 등

하지만 여전히 지원하지 않는 값들도 있다.

- `Error`
- `Function`
- `DOM node`

좀 더 자세한 정보를 보고 싶다면 아래 링크를 참조하자.

> https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API/Structured_clone_algorithm

```js
const a = {
  date: new Date(), // stringified
  undef: undefined, // lost
  inf: Infinity, // forced to 'null'
  re: /.*/, // lost
}

const deepCopyA = structuredClone(a)

console.log(deepCopyA)

// {
//   date: 2022-08-31T00:35:59.469Z,
//   undef: undefined,
//   inf: Infinity,
//   re: /.*/
// }
```

# :books:참고자료

https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript/5344074#5344074

https://medium.com/@pmzubar/why-json-parse-json-stringify-is-a-bad-practice-to-clone-an-object-in-javascript-b28ac5e36521

https://developer.mozilla.org/ko/docs/Web/API/Web_Workers_API/Structured_clone_algorithm