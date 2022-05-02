# 모던 자바스크립트

## 챕터 : 재귀와 스택

### 과제1

```js
function sumTo(n) {
  let sum = 0;
  for (let i = 1; i <= n; i++) {
    sum += i;
  }
  return sum;
}

function sumTo2(n) {
  let sum = 0;
  if (n == 1) {
    sum += n;
  } else {
    sum += sumTo2(n - 1);
  }
  return sum;
}

function sumTo3(n) {
  return (n * (n + 1)) / 2;
}
```

1. 반복문을 활용

2. 재귀 사용

3. 등차수열 공식 사용

총 3가지 방법으로 100까지의 합을 구했다. 등차수열 공식을 사용하는 경우가 제일 빠른데, 그 이유는 n에 상관없이 오직 세 개의 연산만 수행하면 되기 때문이다.

반복문을 사용하는 방법은 두 번째로 빠른데, 그 이유는 재귀를 사용할 경우 중첩 호출과 실행 스택 관리가 추가로 필요하기 때문이다.

#### 과제2

```js
function factorial(n) {
  if (n == 1) {
    return 1;
  } else {
    return n * factorial(n - 1);
  }
}
```

재귀를 활용해 팩토리얼을 구현했다.

#### 과제3
