# 함수형 프로그래밍

## 활용

다음은 명령형 프로그래밍 방식으로 작성된 n개의 홀수를 제곱한 후, 그 값들의 합을 구하는 함수이다.

```js
function f1(limit, list) {
  let sum = 0;
  for (const a of list) {
    if (a % 2) {
      const b = a * a;
      sum += b;
      if (--limit == 0) {
        break;
      }
    }
  }
  console.log(sum);
}
```

이를 함수형 프로그래밍 방식으로 변환시켜보자.

## if를 filter로

```js
function f2(limit, list) {
  let sum = 0;
  for (const a of list.filter(a=>a%2)) {
      const b = a * a;
      sum += b;
      if (--limit == 0) {
        break;
      }
    }
  }
  console.log(sum);
}
```

> **filter()** 메서드는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환합니다.

아까는 if 문을 통해 전달된 배열 전체에서 홀수에 대해서 연산을 진행했다면, 이번에는 전달된 배열 자체를 변형시키는 것이다.  

## 값 변화 후 변수 할당을 map으로

```js
function f3(limit, list) {
  let sum = 0;
  for (const a of list.filter((a) => a % 2).map((a) => a * a)) {
    sum += a;
    if (--limit == 0) {
      break;
    }
  }
  console.log(sum);
}
```

> **map()** 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

filter 메서드를 통해 홀수만 걸러낸 배열의 요소들을 map 메서드를 사용해 원하는 형태로 바꿔준다. 이번 경우에는 각 요소들을 모두 제곱했다.

# :books:참고자료

- 인프런 함수형 프로그래밍과 JavaScript ES6+ 응용편 강의
