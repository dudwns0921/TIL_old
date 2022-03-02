# ES6 함수의 추가 기능

## 화살표 함수

화살표 함수는 function 키워드 대신 화살표를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.

화살표 함수는 표현뿐만 아니라 내부 동작도 기존의 함수보다 간략하다.

### 함수 정의

```javascript
const add = (x, y) => x+y;
add(3,5); // -> 8
```

### 매개변수 선언

- **매개 변수가 여러 개인 경우 소괄호 안에 매개변수를 선언**

```javascript
const arrow = (x, y) => {...};
```

- **매개 변수가 한 개인 경우 소괄호 생략 가능**

```javascript
const arrow = x => {...};
```

- **매개 변수가 없을 경우 소괄호 생략 불가**

```javascript
const arrow = () => {...};
```

### 함수 몸체 정의

- **함수 몸체가 하나의 문일 경우 함수 몸체를 감싸는 중괄호 생략 가능**

이 때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.

이를 implicit return이라고 한다.

```javascript
const arrow = x => x**2;
arrow(2); // -> 4
```

표현식이 아닌 경우 에러가 발생한다.

```javascript
const arrow = () => const x =1; // Uncaught SyntaxError: Unexpected token 'const'
```

- **객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호로 감싸 주어야 함**

```javascript
const arrow = (id, content) => ({id, content})
arrow(1, "Javascript"); // -> {id: 1, content: "Javascript"}
```

그렇지 않으면 객체 리터럴을 감싸는 중괄호를 함수 몸체를 감싸는 중괄호로 잘못 해석한다.

```javascript
const arrow = (id, content) => {id, content}
arrow(1, "Javascript"); // -> undefined
```

```
const button = document.querySelector("button");

button.addEventListener("click", function() {
    console.log(this);
    console.log("I've been clicked");
})
```

### this

화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.

따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라고 한다.

사실 이 부분을 이해하기 위해서는 this, 스코프 등에 관련된 많은 지식들이 필요하므로 간단한 예시를 통해 살짝만 살펴보자.

```javascript
const button = document.querySelector("button");

button.addEventListener("click", function(){
    console.log(this); // <button>Click Me</button>
})
```

위 예제에서 this는 이벤트리스너가 등록된 html 요소인 button을 반환한다.

```javascript
const button = document.querySelector("button");

button.addEventListener("click", ()=>{
    console.log(this); // Window
})
```

화살표 함수를 사용하면 Window 객체를 반환한다.

위 예제에서 상위 스코프는 제일 바깥 범위인 Window 객체가 되기 때문이다.

위에서 말했듯이 이 부분을 완벽하게 이해하는 건 쉽지 않다.

지금은 그냥 화살표 함수를 사용하게 되면 this는 상위 스코프의 this를 참조하게 되므로

this를 사용하고 싶다면 일반 함수를 사용한다는 정도로만 알아두자.

## 매개변수 기본값

매개변수에 기본값을 설정할 수 있다. 

```javascript
function sum(x=0, y=0) {
    return x+y;
}

console.log(sum(1)); // NaN
```

인수가 전달되지 않은 매개변수의 값은 undefined다.

이를 방치하면 위와 같이 의도치 않은 결과가 나올 수 있다.

```javascript
function sum(x=0, y=0) {
    return x+y;
}

console.log(sum(1,2)); // 3
console.log(sum(1)); // 1
```

기본값은 위와 같이 변수의 값을 할당하듯이 매개변수에 값을 할당해주면 된다.

# :books:참고자료

이웅모, 모던 자바스크립트 Deep Dive, 위키북스, 2020

노마드코더 수업 내용