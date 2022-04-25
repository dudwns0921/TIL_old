# 타입스크립트

## 타입스크립트란?

타입스크립트는 자바스크립트에 타입을 부여한 언어다. 자바스크립트의 확장된 언어라고 볼 수 있다. 타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 파일을 한 번 컴파일해야 한다.



## 타입스크립트를 사용하는 이유

- 에러의 사전 방지

- 코드 가이드 및 자동완성(개발 생산성 향상)

### 에러의 사전 방지

```javascript
// math.js
function sum(a, b) {
  return a + b;
}
```

```typescript
// math.ts
function sum(a: number, b: number) {
  return a + b;
}
```

위 2개의 코드를 비교해 어떻게 에러를 방지할 수 있는지 살펴보자. 두 코드 모두 두 숫자의 합을 구하는 함수 코드이다. 그렇기에 a와 b에는 당연히 number 타입의 인자가 들어와야 할 것이다. 하지만 실수로 다른 타입을 넣을 수도 있는데, 이 때 우리는 그 결과를 함수가 실행된 후에야 알 수 있게 된다.

```javascript
sum('10', '20'); // 1020
```

여기서 타입스크립트를 사용해 매개변수의 타입을 결정해 놓는다면 다음과 같은 오류 메시지를 통해 의도치 않은 코드의 동작을 예방할 수 있다.

![sum 함수 타입 에러](https://joshua1988.github.io/ts/images/type-error.png)

### 코드 자동 완성과 가이드

타입스크립트의 또 다른 장점은 코드를 작성할 때 개발 툴의 기능을 최대로 활용할 수 있다는 것이다. 요즘에 프런트엔드 개발을 할 때 가장 많이 사용되는 Visual Studio Code는 툴의 내부가 타입스크립트로 작성되어 있어 타입스크립트 개발에 최적화 되어 있다.

개발자 관점에서 자바스크립트에 타입이 더해졌을 때 어떠한 장점이 있는지 살펴보기 위해 아래 자바스크립트 코드를 보자.

```javascript
// math.js
function sum(a, b) {
  return a + b;
}
var total = sum(10, 20);
total.toLocaleString();
```

위 코드는 앞에서 살펴봤던 `sum()` 함수를 이용하여 두 숫자의 합을 구한 다음 `toLocaleString()`(특정 언어의 표현 방식에 맞게 숫자를 표기하는 API)를 적용한 코드이다.

여기서 주목해야 할 부분은 같이 코드를 작성하는 시점에는 자바스크립트가 `total`이라는 변수의 타입을 인지하지 못한다는 점이다.

![변수 타입이 없을 때의 코딩](https://joshua1988.github.io/ts/images/math-js.gif)에서 볼 수 있듯이 `total`이라는 변수의 타입이 정해져 있지 않기 때문에 자바스크립트 Number에서 제공하는 API인 `toLocaleString()`을 일일이 작성했다. 타입스크립트를 통해 작성한다면 어떻게 될까?

```typescript
function sum(a: number, b: number): number {
  return a + b;
}
var total = sum(10, 20);
total.toLocaleString();
```

![변수의 타입이 있을 때의 코딩](https://joshua1988.github.io/ts/images/math-ts.gif)

변수 `total`에 대한 타입이 지정되어 있기 때문에 VSCode에서 해당 타입에 대한 API를 미리 보기로 띄워줄 수 있고 따라서, API를 다 일일이 치는 것이 아니라 tab으로 빠르고 정확하게 작성해나갈 수 있다.

# :books:참고자료

[Why TypeScript? | 타입스크립트 핸드북](https://joshua1988.github.io/ts/why-ts.html#%EC%99%9C-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8D%A8%EC%95%BC%ED%95%A0%EA%B9%8C%EC%9A%94)
