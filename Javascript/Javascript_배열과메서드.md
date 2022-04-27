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

과제에 대한 답만 적어놓았기 때문에 아래 링크와 함께 보는 것을 추천한다.

> https://ko.javascript.info/array-methods#ref-400

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

### 과제4

### 배열 복사본을 정렬하기

 **`*원본 배열은 변경되지 말아야 함`**

```javascript
function copySorted(arr) {
  return arr.slice().sort();
}
```

### 과제5

### 이름 매핑하기

```js
function mapName(users) {
  return users.map((element) => element.name);
}
```

### 과제6

### 객체 매핑하기

```js
function mapObject(users) {
  return users.map((element) => {
    return {
      fullName: [element.name, element.surname].join(' '),
      id: element.id,
    };
  });
}
```

### 과제7

### 나이를 기준으로 객체 정렬하기

```js
function sortByAge(arr) {
  arr.sort((a, b) => a.age - b.age);
}
```

sort의 특징들을 간단하게 다시 정리해보겠다.

1. 매개변수를 생략할 경우 요소들은 ASCll 문자 기준 오름차순으로 정렬된다.

    숫자를 입력해도 ASCll 기준으로 정렬하기 위해 string으로 형변환이 일어난다.

2. 매개변수로 비교함수를 넘겨 자신이 원하는 방식대로 정렬시킬 수 있다.

    비교하는 요소들을 각각 a,b라고 할 때, 반환값에 따른 sort 함수의 해석은 아래와 같다.

- 반환 값 < 0 : a가 b보다 앞에 있어야 한다.

- 반환 값 = 0 : a의 b의 순서를 바꾸지 않는다.

- 반환 값 > 0 : b가 a보다 앞에 있어야 한다. 

### 과제8

### 평균 나이 구하기

```js
function getAverage(arr) {
  let sum = 0;
  const ages = arr.map((element) => {
    return element.age;
  });
  for (let i of ages) {
    sum += i;
  }
  return sum / ages.length;
}

function getAverage2(arr) {
  let sum = 0;
  arr.array.forEach((element) => {
    sum += element.age;
  });
  return sum / arr.length;
}

function getAverage3(arr) {
  let sum = 0;
  arr.array.forEach((element) => {
    sum += element.age;
  });
  return sum / arr.length;
}
```

여러 방법들을 생각해봤지만 역시나 해답이 가장 간결했다.

```js
function getAverageAge(users) {
  return users.reduce((prev, user) => prev + user.age, 0) / users.length;
}
```

해답에서는 reduce 메서드를 사용했는데, reduce는 배열을 기반으로 값 하나를 도출할 때 사요되는 메서드이다. 

### 과제9

### 중복 없는 요소 찾아내기

```js
let strings = [
  'Hare',
  'Krishna',
  'Hare',
  'Krishna',
  'Krishna',
  'Krishna',
  'Hare',
  'Hare',
  ':-O',
];

function unique(arr) {
  let map = new Map();
  let answer;
  for (let item of arr) {
    if (map.has(item)) {
      map.set(item, map.get(item) + 1);
    } else {
      map.set(item, 0);
    }
  }
  map.forEach(function (value, key) {
    if (value === 0) {
      answer = key;
    }
  });
  return answer;
}
```

문제가 좀 모호해서 잘못된 해답을 작성했다. 중복되지 않는 요소를 찾아서 반환하는 문제인줄 알았는데, 알고 보니 중복 없이 요소들 하나씩을 반환하는 문제였다. 더불어 배열과 메서드를 활용하지 않아 이 챕터의 과제에 대한 답변으로는 적당하지 않다. 그래도 풀이 과정을 살펴보자면,

1. key와 value 형태로 요소들을 저장하는 자료구조인 map을 생성한다.

2. 중복된 key가 있으면 value를 1 증가시키고, 중복된 키가 없으면 0을 value로 해서 map에 넣는다.

3. forEach 함수를 사용해서 모든 요소를 순회하며 중복되지 않은 요소, 그러니까 value가 0인 요소를 찾아 해당 요소의 키를 반환한다.

문제의 본래 목적대로 푼다면,

```js
function unique(arr) {
  let result = [];

  for (let str of arr) {
    if (!result.includes(str)) {
      result.push(str);
    }
  }

  return result;
}
```

1. 새로운 결과 배열을 만든다.

2. 배열에 넣으려는 요소가 배열에 없을 경우에만 요소를 추가한다.

### 과제10

### Create keyed object from array

```js
function groupById(users) {
  let obj = {};
  users.map((element) => {
    obj[element.name] = element;
  });
  return obj;
}
```

앞에서 선언한 obj 객체에 user의 이름을 key로 갖고, user 객체를 value로 갖는 객체를 추가하는 방식으로 문제를 풀었다. 해답에서는 reduce 메서드를 사용하라고 되어있는데, 아무래도 reduce 메서드의 사용방법이 어렵게 느껴져서 아직은 사용하기가 좀 망설여진다.

```js
function groupById(array) {
  return array.reduce((obj, value) => {
    obj[value.id] = value;
    return obj;
  }, {})
}
```

# :books:참고자료

https://ko.javascript.info/array-methods#ref-400


