# Useful Array Methods

## of()

`Array.of()` 메서드는 인자의 수나 유형에 관계없이 가변 인자를 갖는 새 `Array` 인스턴스를 만든다.

`Array.of()`와 `Array` 생성자의 차이는 정수형 인자의 처리 방법에 있다.

`Array.of(7)`은 하나의 요소 `7`을 가진 배열을 생성하지만 `Array(7)`은 `length` 속성이 7인 빈 배열을 생성한다.

```javascript
Array.of(7);       // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```

## from()

`Array.from()` 메서드는 유사 배열 객체(array-like object)나 반복 가능한 객체(iterable object)를 얕게 복사해 새로운`Array` 객체를 만든다.

예시를 통해 살펴보자.

```html
...
<body>
    <button>1</button>
    <button>2</button>
    <button>3</button>
    <button>4</button>
    <button>5</button>
    <button>6</button>
    <button>7</button>
    <button>8</button>
    <button>9</button>
    <button>10</button>
</body>
...
```

```javascript
const buttons = document.getElementsByTagName("button");

console.log(buttons); / HTMLCollection(10) [button, button, button, button, button, button, button, button, button, button]
```

위의 HTMLCollection은 얼핏 보면 array 같아 보이지만 유사 배열 객체이다.

buttons에다가 일괄적으로 이벤트 리스너를 추가하고 싶을 때 배열이라면 이렇게 할 것이다.

```javascript
buttons.forEach(button => {
    button.addEventListener("click", ()=>console.log("I've been clicked!"));

});

// Uncaught TypeError: buttons.forEach is not a function

Array.from(buttons).forEach(button => {
    button.addEventListener("click", ()=>console.log("I've been clicked!"));

});
```

buttons는 배열이 아니기 때문에 forEach 메소드를 가지고 있지 않다. Array.from()을 사용하면 유사 배열 객체를 배열로 바꿀 수 있다.

## find()

`find()` 메서드는 주어진 판별 함수를 만족하는 **첫 번째 요소**의 **값**을 반환한다. 그런 요소가 없다면 [`undefined`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)를 반환한다.

```javascript
const friends = [
    "Jun@korea.com",
    "Lee@gmail.com",
    "Nico@naver.com",
    "Lynn@hanmail.net",
    "Rose@kakao.com"
];

const target = friends.find(friend => friend.includes("Jun"));
const wrongTarget = friends.find(friend => friend.includes("Anonymous"));

console.log(target); // Jun@korea.com
console.log(wrongTarget); // undefined
```

## findIndex()

**`findIndex()`** 메서드는 **주어진 판별 함수를 만족하는** 배열의 첫 번째 요소에 대한 **인덱스**를 반환한다. 만족하는 요소가 없으면 -1을 반환한다.

```javascript
const friends = [
    "Jun@korea.com",
    "Lee@gmail.com",
    "Nico@naver.com",
    "Lynn@hanmail.net",
    "Rose@kakao.com"
];

const target = friends.findIndex(friend => friend.includes("Jun"));
const wrongTarget = friends.findIndex(friend => friend.includes("Anonymous"));

console.log(target); // 0
console.log(wrongTarget); // -1
```

findIndex 함수는 배열 안에 요소를 수정하고 싶을 때 유용하다.

```javascript
const friends = [
    "Jun@gorea.com", // korea.com으로 고쳐서 출력해보기
    "Lee@gmail.com",
    "Nico@naver.com",
    "Lynn@hanmail.net",
    "Rose@kakao.com"
];

const check = () => friends.findIndex((friend) => friend.includes("gorea.com"));

let target = check();

if (target !== -1) {
const username = friends[target].split("@")[0];

const email = "korea.com";

friends[target] = `${username}@${email}`;
}

console.log(friends);
```

## fill()

`fill()` 메서드는 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채운다.

```javascript
const array1 = [1, 2, 3, 4];

// 인덱스 2부터 3까지 0으로 채우기
console.log(array1.fill(0, 2, 3));
// [1, 2, 0, 0]

// 인덱스 1부터 끝까지 5로 채우기
console.log(array1.fill(5, 1));
// [1, 5, 5, 5]

console.log(array1.fill(6));
// [6, 6, 6, 6]
```

# :books:참고자료

이웅모, 모던 자바스크립트 Deep Dive, 위키북스, 2020

노마드코더 수업 내용 