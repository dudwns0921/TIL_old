# Functional Programming

## 제너레이터와 이터레이터

- 제너레이터 : 이터레이터이자 이터러블을 생성하는 함수

```js
function* gen1() {
  yield 1;
  yield 2;
  yield 3;
}
let iter = gen1();
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
// { value: 1, done: false }
// { value: 2, done: false }
// { value: 3, done: false }
// { value: undefined, done: true }
```

제너레이터 함수는 일반 함수에 *을 붙여서 만든다.

위와 같은 방법으로 제너레이터 함수를 통해 쉽게 이터러블을 만들 수 있다.

## :bulb:Tip - yield란?

`yield 키워드`는 아래와 같은 역할을 한다.

- 제너레이터 함수의 실행을 중지
- 그리고 `yield 키워드` 뒤에오는 표현식[expression]의 값은 제너레이터의 caller로 반환

​	* 제너레이터 버전의 `return` 키워드로 생각할 수 있다.

___

```js
console.log(iter[Symbol.interator]() == iter);
// true
```

[Symbol.iterator] 메서드의 반환 결과가 자기 자신이기 때문에 제너레이터 함수는 well-formed 이터러블을 반환하는 함수이다. 그렇기 때문에 for...of 문을 사용할 수 있다.

```js
for(const a of gen()) {
	console.log(a)
}
// 1
// 2
// 3
```

```js
function* gen2() {
  // gen1()과 동일
  return 1000;
}
let iter = gen2()
// 전부 console.log()할 경우
// { value: 1, done: false }
// { value: 2, done: false }
// { value: 3, done: false }
// { value: 1000, done: true }
```

제너레이터 함수에 리턴값을 작성하면, done이 true일 때 해당 값을 value로 받을 수 있다. 하지만 for...of 문을 사용해도 해당 값을 순회하지는 않는다. 

```js
function* gen3() {
  // gen1()과 동일
  if(false) yield 3;
  return 1000;
}
// 전부 console.log()할 경우
// { value: 1, done: false }
// { value: 2, done: false }
// { value: 1000, done: true }
```

추가적으로, 다음과 같이 조건문을 사용하면 순회할 때 해당 값을 생략할 수도 있다. 쉽게 말하자면, 제너레이터는 표현식을 통해 값을 만들 수 있고, 그 값을 순회할 수 있도록 할 수 있다. 아래의 예시들을 살펴보자.

```js
function* odds(l) {
  for (let i = 1; i < l; i++) {
    if (i % 2 !== 0) {
      yield i;
    }
  }
}
let iter = gen(10);
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
// { value: 1, done: false }
// { value: 3, done: false }
// { value: 5, done: false }
// { value: 7, done: false }
// { value: 9, done: false }
```

```js
function* infinity(i = 0) {
  while (true) {
    yield i++;
  }
}
let iter = infinity();
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
// { value: 0, done: false }
// { value: 1, done: false }
// { value: 2, done: false }
// { value: 3, done: false }
// { value: 4, done: false }
// ... next() 메서드로 무한히 진행 가능
```

# :books:참고자료

인프런 함수형 프로그래밍과 JavaScript ES6+ 강의
