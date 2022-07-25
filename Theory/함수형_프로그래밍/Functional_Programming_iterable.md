# Functional Programming

## 순회와 이터러블

### 기존 방식

```js
const list = [1,2,3,4]
for(let i=0; i<list.length; i++) {
	console.log(list[i])
}
```

### ES6 방식

```js
const arr = [1, 2, 3, 4];
for (const a of arr) {
  console.log(a);
}
```

for...of문을 사용하면 array뿐만 아니라 자바스크립트 내에 구현된 Set, Map도 순회가 가능하다. 그런데 만약 for...of문이 기존 방식과 같이 구현되어 있다면 Set과 Map을 순회하는 건 불가능하다.

```js
console.log(set[0])
// undefined
```

set은 순서가 없는 자료형이기 때문에 인덱스를 통해서 특정 요소를 가져올 수 없다. 따라서 for...of문은 다른 방법으로 순회가 이루어지고 있다는 걸 알 수 있다.

### for...of문을 사용하기 위한 조건

```js
const arr = [1, 2, 3, 4];
arr[Symbol.iterator] = null;
for (const a of arr) {
  console.log(a);
}
// TypeError: arr is not iterable
```

위 예시를 통해 for...of문을 사용하기 위해서는 arr가 이터러블이어야 한다는 사실을 확인 가능하다.

## :bulb:Tip - 이터러블, 이터레이터, 이터러블/이터레이터 프로토콜

**이터러블 이터레이터 프로토콜** : 이터러블을 for of 문, 전개 연산자 등과 함께 동작하도록 한 규약

**이터러블** : 기본 반복자(default iterator)를 리턴하는 Symbol.iterator()을 가진 값

**이터레이터** : {value, done} 객체를 리턴하는 next() 를 가진 값

___

이터러블이 되기 위해서는 객체의 이터레이터를 반환하는 메서드인 Symbol.iterator()를 가지고 있어야 한다. 위의 내용들을 참고해 이터러블을 만들어보겠다.

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 1;
    return {
      next() {
        return i == 5 ? { done: true } : { value: i++, done: false };
      },
    };
  },
};
for (const a of iterable) {
  console.log(a);
}
// 1
// 2
// 3
// 4
```

정상적으로 for...of문으로 순회가 되는 것을 확인할 수 있다. 하지만 이 이터러블은 well-formed 이터러블이라고 할 수 없다. 반환하는 이터레이터가 자기 자신을 반환하는 symbol.iterator 메서드를 가지고 있을 때 well-formed iterable이라고 할 수 있다.

### Well-formed iterable

well-formed 이터러블의 장점은 이터러블이 어디까지 순회했는지 기억하고 있다는 점이다. 아래 예시를 보자.

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 1;
    return {
      next() {
        return i == 5 ? { done: true } : { value: i++, done: false };
      },
      [Symbol.iterator]() {
        return this;
      },
    };
  },
};

let iterator = iterable[Symbol.iterator]();
iterator.next();

for (const a of iterator) {
  console.log(a);
}
// 2
// 3
// 4
```

1. 이터러블의 Symbol.iterator 메서드를 통해 이터레이터를 반환한다.
2. 이터레이터가 갖고 있는 next 메서드를 통해 순회를 한 번 진행한다.
3. 해당 이터레이터를 다시 for...of문에 넣고 순회하면 2,3,4가 출력된다.

### 전개 연산자

전개 연산자 또한 이터러블일 때만 사용이 가능하다.

```js
const arr = [1, 2, 3, 4];
arr[Symbol.iterator] = null;
console.log([...arr])
// TypeError: arr is not iterable
```

# :books:참고자료

인프런 함수형 프로그래밍과 JavaScript ES6+ 강의

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol#%EC%84%A4%EB%AA%85description
