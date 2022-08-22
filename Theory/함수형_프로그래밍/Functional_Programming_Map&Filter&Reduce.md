# Functional Programming

## Map 파헤치기

> **`map()`** 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

`map`메서드를 실제로 만들어보면서 어떻게 구성되어있는지 파헤쳐보자.

```js
const products = [
  { name: "반팔티", price: 15000 },
  { name: "긴팔티", price: 20000 },
  { name: "핸드폰케이스", price: 15000 },
  { name: "후드티", price: 30000 },
  { name: "바지", price: 25000 },
];
```

위 상품 목록에서 name, 그리고 price의 값들만 따로 모아서 배열로 반환해보도록 하겠다. 먼저 명령형 프로그래밍으로 작성해보자.

```js
let names = [];

for (const product of products) {
  names.push(product.name);
}

console.log(names);
// [ '반팔티', '긴팔티', '핸드폰케이스', '후드티', '바지' ]

let prices = [];

for (const product of products) {
  prices.push(product.price);
}

console.log(prices);
// [ 15000, 20000, 15000, 30000, 25000 ]
```

이제 동일한 작업을 수행하는 `customMap`함수를 만들어보겠다.

```js
const customMap = (f, iter) => {
  let res = [];
  for (const item of iter) {
    res.push(f(item));
  }
  return res;
};

const returnName = (item) => item.name;

console.log(customMap(returnName, products));
// [ '반팔티', '긴팔티', '핸드폰케이스', '후드티', '바지' ]
```

1. `f`, 주어진 함수가 반환하는 값을 배열 `res`에다가 `push`한다.

위의 예시에서는 객체 안의 `name`이라는 속성을 반환하는 함수를 전달하고 있지만,  함수의 반환값만 바꿔주면 원하는 값을 수집해서 배열로 반환받을 수 있다.

## Filter 파헤치기

> **`filter()`** 메서드는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환합니다.

`filter`메서드를 실제로 만들어보면서 어떻게 구성되어있는지 파헤쳐보자.

```js
const products = [
  { name: "반팔티", price: 15000 },
  { name: "긴팔티", price: 20000 },
  { name: "핸드폰케이스", price: 15000 },
  { name: "후드티", price: 30000 },
  { name: "바지", price: 25000 },
];
```

앞에서 `Map`을 살펴볼 때 사용한 상품 목록이다. 여기서 가격이 20000원 미만인 상품들만 걸러내보도록 하겠다. 먼저 명령형 프로그래밍 방식으로 작성해보자.

```js
let under20000 = [];
for (const product of products) {
  if (product.price < 20000) {
    under20000.push(product);
  }
}

console.log(under20000);
// [ { name: '반팔티', price: 15000 }, { name: '핸드폰케이스', price: 15000 } ]
```

if 문을 통해 간단하게 처리했다. 이제 동일한 작업을 수행하는 `customFilter`함수를 만들어보겠다. 

```js
const customFilter = (f, iter) => {
  let res = [];
  for (const item of iter) {
    if (f(item)) res.push(item);
  }
  return res;
};

console.log(customFilter((p) => p.price < 20000, products));
// [ { name: '반팔티', price: 15000 }, { name: '핸드폰케이스', price: 15000 } ]
```

1. `f`, 주어진 함수는 전달 받은 값이 테스트를 통과하는지 안하는지에 따라 `true` 혹은 `false`를 반환한다.
2. 테스트를 통과해 true가 반환될 경우, if문을 통해 테스트를 통과한 값들만 배열에 `push`한다.

## Reduce 파헤치기

> **`reduce()` **메서드는 배열의 각 요소에 대해 주어진 **리듀서**(reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.

 `reduce`메서드를 실제로 만들어보면서 어떻게 구성되어있는지 파헤쳐보자.

___

먼저 명령형 프로그래밍으로 주어진 배열의 값을 모두 누적해보도록 하겠다.

```js
const nums = [1, 2, 3, 4, 5];

let total = 0;

for (const num of nums) {
  total += num;
}

console.log(total); // 15
```

`reduce`를 사용하면 다음과 같이 수정할 수 있다.

```js
let total = 0;

total = nums.reduce((a, b) => a + b, total);

console.log(total) // 15
```

`reduce`메서드는 위와 같이 리듀서 함수와 초기값을 인수로 받는다. 따라서 지금부터 만들어볼 `customReduce`함수는 다음과 같은 형태를 가지며, 실제 `reduce`를 사용했을 때와 동일한 결과를 반환한다.

```js
const customReduce = (f, acc, iter) => {
  // 작업 수행
}

console.log(customReduce(add, 0, [1,2,3,4,5])) // 15
```

리듀서 함수와 `customReduce`의 내부로직을 작성해주자.

```js
const add = (a,b) => a+b;
const customReduce = (acc, init, iter) => {
  for (const n of iter) {
    acc = f(acc, n);
  }
  return acc;
}

console.log(customReduce(add, 0, [1,2,3,4,5])) // 15
```

`reduce`메서드와 얼추 비슷한 함수가 완성되었다. 좀 더 완성도를 높이기 위해 `reduce` 메서드가 갖고 있는 특징을 적용시켜보자. 먼저 `reduce`메서드는 다음과 같은 특징을 가진다.

> `initialValue`를 제공하지 않으면, `reduce`메서드는 인덱스 1부터 시작해 콜백 함수(리듀서 함수)를 실행하고 첫 번째 인덱스는 건너 뜁니다. `initialValue`를 제공하면 인덱스 0에서 시작합니다.

정리하자면 초기값이 없을 때, 주어진 배열의 첫 번째 값을 초기값을 사용한다는 의미이다. 실제로 구현해보도록 하자.

```js
const customReduce = (f, acc, iter) => {
  if (!iter) {
  // 인수가 2개 주어진 경우 => iter가 undefined인 상태
    iter = acc[Symbol.iterator]();
  	// 인수가 2개이므로 acc는 iter의 값을 가짐
    // [Symbol.iterator] 메서드를 통해 iterable을 반환해 iter에 할당
    acc = iter.next().value;
    // 할당된 iter의 첫번째 값을 acc에 할당
  }

  for (const n of iter) {
    acc = f(acc, n);
  }
  return acc;
};

console.log(customReduce(add, [1, 2, 3, 4, 5])); // 15
```

마지막으로 `reduce`메서드는 누산하는 과정을 인수로 전달받는 리듀서 함수에 완전히 위임하고 있어, 리듀서 함수만 바꿔준다면 숫자 배열 외에 다른 복잡한 자료를 순회하며 누산하는 것에 아무런 어려움이 없다. 다음 예시를 살펴보자.

```js
const products = [
  { name: "반팔티", price: 15000 },
  { name: "긴팔티", price: 20000 },
  { name: "핸드폰케이스", price: 15000 },
  { name: "후드티", price: 30000 },
  { name: "바지", price: 25000 },
];

console.log(
  customReduce(
    (total_price, product) => {
      return total_price + product.price;
    },
    0,
    products
  )
); // 105000
```

## 마무리

`map`, `filter`, `reduce` 모두 실무에서 정말 많이 사용하는 메서드들이다. 개인적인 분석이기 때문에 오류가 있을 수도 있지만 이 셋의 공통점은 다음과 같다고 생각한다.

- 고차함수
- 다형성 지원

세 가지 메서드 모두 함수를 인자로 사용하고 있기 때문에 고차함수의 특징을 가진다. 또한 인자로 사용하는 함수에 따라 다양한 형태의 자료에 대한 작업이 가능하기 때문에 다형성을 지원한다고 할 수 있다.

# :books:참고자료

인프런 함수형 프로그래밍과 JavaScript ES6+ 강의

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce