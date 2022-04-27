# 모던 자바스크립트

## 챕터 : 배열과 메서드

### 요소를 더하거나 지우기

- `push(...items)` – 맨 끝에 요소 추가하기
- `pop()` – 맨 끝 요소 추출하기
- `shift()` – 첫 요소 추출하기
- `unshift(...items)` – 맨 앞에 요소 추가하기
- `splice(pos, deleteCount, ...items)` – `pos`부터 `deleteCount`개의 요소를 지우고, `items` 추가하기
- `slice(start, end)` – `start`부터 `end` 바로 앞까지의 요소를 복사해 새로운 배열을 만듦
- `concat(...items)` – 배열의 모든 요소를 복사하고 `items`를 추가해 새로운 배열을 만든 후 이를 반환함. `items`가 배열이면 이 배열의 인수를 기존 배열에 더해줌

### 원하는 요소 찾기

- `indexOf/lastIndexOf(item, pos)` – `pos`부터 원하는 `item`을 찾음. 찾게 되면 해당 요소의 인덱스를, 아니면 `-1`을 반환함
- `includes(value)` – 배열에 `value`가 있으면 `true`를, 그렇지 않으면 `false`를 반환함
- `find/filter(func)` – `func`의 반환 값을 `true`로 만드는 첫 번째/전체 요소를 반환함
- `findIndex`는 `find`와 유사함. 다만 요소 대신 인덱스를 반환함

### 배열 전체 순회하기

- `forEach(func)` – 모든 요소에 `func`을 호출함. 결과는 반환되지 않음

### 배열 변형하기

- `map(func)` – 모든 요소에 `func`을 호출하고, 반환된 결과를 가지고 새로운 배열을 만듦
- `sort(func)` – 배열을 정렬하고 정렬된 배열을 반환함
- `reverse()` – 배열을 뒤집어 반환함
- `split/join` – 문자열을 배열로, 배열을 문자열로 변환함
- `reduce(func, initial)` – 요소를 차례로 돌면서 `func`을 호출함. 반환값은 다음 함수 호출에 전달함. 최종적으로 하나의 값이 도출됨

### 기타

- `Array.isArray(arr)` – `arr`이 배열인지 여부를 판단함

`sort`, `reverse`, `splice`는 기존 배열을 변형시킨다는 점에 주의하시기 바랍니다.

### 과제1

### border-left-width를 borderLeftWidth로 변경하기

```js
function camelize(str) {
  return str
    .split('-')
    .map((word, index) => {
      return index == 0 ? word : word[0].toUpperCase() + word.slice(1);
    })
    .join('');
}
```

1. 먼저 split 함수를 통해 '-'을 기준으로 나눠지는 요소들로 만든 배열을 반환받는다.
   
   가령 인자로 background-color을 받았을 경우, 반환받는 배열은 ['background', 'color']가 된다.

2. 그 다음 map 메서드를 통해 첫 번째 요소를 제외한 나머지 요소들의 첫 번째 문자를 대문자로 바꿔준다.
   
   이 과정까지의 반환값은 ['Background', 'Color']이다.

3. 그리고 join 메서드를 통해 하나의 문자열로 만들어준다.

### 과제2

### 특정 범위에 속하는 요소 찾기

**`*원본 배열은 변경되지 말아야 함`**

```js
function filterRange(arr, range1, range2) {
  return arr
    .map((element) => {
      if (element >= range1 && element <= range2) {
        console.log(element);
        return element;
      }
    })
    .filter((element) => element != undefined);
}
```

처음에는 map 메서드를 통해 조건에 맞는 요소들만 반환받아 새로운 배열을 만들려고 했었다. 그런데 요소가 조건에 해당하지 않아 return 구문이 없을 경우에도 undefined가 반환되는 걸 확인했다. 그래서 filter 메서드를 통해 undefined 인 요소들을 모두 걸러냈다.

사실 해답은 훨씬 간단했다. filter 함수를 통해 바로 조건에 맞는 배열을 반환했다.

```javascript
function filterRange(arr, a, b) {
  return arr.filter(item => (a <= item && item <= b));
}
```

그렇다면 어떤 게 맞는 방법일까? 사실 한 눈에 보기에도 모던 자바스크립트 쪽의 해답이 훨씬 나아보인다. 그래도 좀 더 구체적인 이유를 찾아보려고 했다.

map 안티패턴

- 새롭게 반환받은 배열을 사용하지 않는 경우

- 콜백 함수로부터 새로운 값을 반환하지 않는 경우

결국 조건에 부합하는 특정 요소들만으로 새로운 배열을 만들기 위해서는 filter을 사용하는 것이 맞다. 

### 과제3

### 특정 범위에 속하는 요소 찾기

**`*원본 배열을 변경해야 함`**

해답에서는 배열을 순회하며 조건에 해당하지 않을 경우 그 요소를 splice를 통해 삭제하는 방법을 사용했다.

```js
function filterRange(arr, range1, range2) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] < range1 || arr[i] > range2) {
      console.log(arr[i]);
      arr.splice(i, 1);
      i--;
    }
  }
}
```

splice로 요소를 삭제한 후에 i의 값에서 1을 빼주는 이유는 splice로 배열의 길이가 줄어들었기 때문이다. 배열의 길이가 줄어들었는데 그냥 검색을 하게 되면 검색해야하는 요소 하나를 넘어가게 된다.

가령 [5,3,8,1], 1, 4 를 인자로 전달했다고 해보자. 5는 1과 4사이의 범위에서 벗어나기 때문에 splice를 통해 삭제된다. 그러면 첫 번째 요소, 인덱스가 0인 값은 3이 되기 때문에 i에서 1을 빼줘야 해당 요소가 조건에 맞는지 확인할 수 있게 된다. 

### 과제3

### 배열 복사본을 정렬하기

 **`*원본 배열은 변경되지 말아야 함`**

```javascript
let arr = ['HTML', 'JavaScript', 'CSS'];

function copySorted(arr) {
  return arr.slice().sort();
}
```


