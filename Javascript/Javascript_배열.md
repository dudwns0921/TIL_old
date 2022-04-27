# 모던 자바스크립트

## 챕터 : 배열

### 과제1

### 배열과 관련된 연산

```js
let styles = ['Jazz', 'Blues'];

styles.push('Rock-n-Roll');

styles[Math.floor((styles.length - 1) / 2)] = 'Classics';
// 가운데 인덱스를 찾기 위해 배열의 길이에서 1을 빼서 최대 인덱스 값을 가져온다.
// 그리고 2로 나눈 후 floor 메서드를 통해 소수점 이하를 버린다.
styles.shift();
// shift는 배열 앞 요소를 제거하고, 제거한 요소를 반환한다.
styles.unshift('Rap', 'Reggae');
// unshift는 배열 앞에 요소를 추가한다.
```

### 과제2

### 배열 컨텍스트에서 함수 호출하기

```js
let arr = ['a', 'b'];

arr.push(function () {
  console.log(this);
});

arr[2](); // [ 'a', 'b', [Function (anonymous)] ]
```

배열이 객체라는 점을 알아야 풀 수 있는 문제이다. `arr[2]()`는 배열인 객체의 메서드를 실행시키는 것과 같다. 따라서 콘솔창에는 this, 즉 객체가 출력된다.

### 과제3

### 최대합 부분 배열

```js
function getMaxSubSum(arr) {
  let maxSum = 0;
// 이중 for문의 결과를 계속 갱신해야 하기 때문에
// for문 스코프의 바깥에 변수를 할당한다.
  for (let i = 0;  < arr.length; i++) {
    let sum = 0;
// for문이 한 바퀴 돌 때마다 sum의 값을 리셋한다. 
    for (let j = i; j < arr.length; j++) {
      sum += arr[j];
      maxSum = Math.max(maxSum, sum);
// 배열을 순회하며 값을 계속 더하면서
// 최대값과 비교해 더 큰 쪽을 maxSum에 할당한다.
    }
  }
  return maxSum;
}
```

getMaxSubSum([-1,2,3,-9])의 과정을 console.log를 통해 순차적으로 출력했다.

```bash
sum 값 리셋, 시작하는 요소의 값 : -1
이번에 더하는 값 : -1
현재 sum의 값 : -1
현재 Maxsum의 값 : 0
이번에 더하는 값 : 2
현재 sum의 값 : 1
현재 Maxsum의 값 : 1
이번에 더하는 값 : 3
현재 sum의 값 : 4
현재 Maxsum의 값 : 4
이번에 더하는 값 : -9
현재 sum의 값 : -5
현재 Maxsum의 값 : 4 

sum 값 리셋, 시작하는 요소의 값 : 2
이번에 더하는 값 : 2
현재 sum의 값 : 2
현재 Maxsum의 값 : 4
이번에 더하는 값 : 3
현재 sum의 값 : 5
현재 Maxsum의 값 : 5
이번에 더하는 값 : -9
현재 sum의 값 : -4
현재 Maxsum의 값 : 5 

sum 값 리셋, 시작하는 요소의 값 : 3
이번에 더하는 값 : 3
현재 sum의 값 : 3
현재 Maxsum의 값 : 5
이번에 더하는 값 : -9
현재 sum의 값 : -6
현재 Maxsum의 값 : 5 

sum 값 리셋, 시작하는 요소의 값 : -9
이번에 더하는 값 : -9
현재 sum의 값 : -9
현재 Maxsum의 값 : 5

5
```

순서가 중요하지 않고, 중복을 허용하지 않으므로 주어진 4개 숫자의 조합(combination)을 확인해보면 된다. 

# :books:참고자료

https://ko.javascript.info/array
