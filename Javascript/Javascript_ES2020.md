# ES2020

노마드코더 ES6의 정석 강의 중 ES2020에 대한 내용들을 정리했다.

## Nullish coalesing operator

디폴트값을 설정할 때 우리는 이항 연산자인 OR 연산자를 이용하곤 한다.

```javascript
console.log("Hello", name || "anonymous");
// Hello anonymous name 변수가 설정되어 있지 않은 경우
```

OR 연산자에는 단점이 하나 있는데, true와 false 값만 구별할 수 있다는 점이다.

```javascript
name = 0;
console.log("Hello", name || "anonymous");
// Hello anonymous
```

name 변수에 0이라는 값을 할당했지만 결과는 여전히 Hello anonymous가 출력된다. OR 연산자는 0을 false로 판단한 것이다.  지금은 name이라는 변수이기 때문에 실제로도 큰 문제는 없겠지만 만약 통화, 점수같은 경우에는 문제가 발생할 것이다.

이런 경우를 위해 만들어진 것이 바로 nullish colaesing operator이다. ?? 연산자는 변수가 오직 null이나 undefined 경우에만 작동한다. 

```javascript
name = 0;
console.log("Hello", name ?? "anonymous");
// Hello 0
```

단순히 참과 거짓을 판단하는 것이 아니라 null과 undefined를 판단하는 연산자이다. 그래서 OR 연산자를 대체한다기보다는 특별한 경우에 ?? 연산자를 사용해준다고 생각하면 된다.



## Optional chaining

optional chaining 연산자는 참조나 기능이 `undefined` 또는 `null`일 수 있을 때 연결된 객체의 값에 접근하는 방법을 제공한다.

예를 들어, 중첩된 구조를 가진 객체인 `obj`가 있다. optional chaining이 없이 깊이 중첩된 하위 속성을 찾으려면, 다음과 같이 참조를 확인해야 한다:

```javascript
let nestedProp = obj.first && obj.first.second;
```

`obj.first`의 값은 `obj.first.second`의 값에 접근하기 전에 `null` (그리고 `undefined`)가 아니라는 점을 검증한다. 이는 `obj.first`를 테스트 없이 `obj.first.second` 에 직접 접근할 때 일어날 수 있는 에러를 방지한다. 

그러나 optional chaining 연산자(`?.`)를 사용한다면 다음과 같이 코드를 작성할 수 있다.

```javascript
let nestedProp = obj.first?.second;
```

`.` 대신에 `?.` 연산자를 사용함으로써, 자바스크립트는 `obj.first.second`에 접근하기 전에 `obj.first`가 `null` 또는 `undefined`가 아니라는 것을 암묵적으로 확인하는 것을 알고 있다. 만약 `obj.first`가 `null` 또는 `undefined`이라면, 그 표현식은 자동으로 단락되어 `undefined`가 반환된다.



# :books: 참고자료

노마드코더 강의 일부


