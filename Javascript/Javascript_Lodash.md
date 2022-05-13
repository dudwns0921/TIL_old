# Lodash

회사의 소스 코드를 분석하다가, `_.find` 라는 처음 보는 코드를 발견했다. 확인해보니 lodash라는 라이브러리를 사용하고 있던 것이었다.

## Lodash란?

 Lodash는 자바스크립트의 배열, 숫자, 객체, 문자열 등을 다루는 데 번거로움을 덜어준다. Lodash의 메서드들은 특히나 아래의 경우에 효과적이다.

- 배열, 객체, 문자열 반복
- value 조작 혹은 검사
- 복합 함수 만들기

### 설치

```bash
$ npm i lodash --save-dev
```

### 사용 방법

공식 문서에서는 아래와 같이 알려주고 있다.

```js
// Load the full build.
var _ = require('lodash');
// Load the core build.
var _ = require('lodash/core');
// Load the FP build for immutable auto-curried iteratee-first data-last methods.
var fp = require('lodash/fp');
 
// Load method categories.
var array = require('lodash/array');
var object = require('lodash/fp/object');
 
// Cherry-pick methods for smaller browserify/rollup/webpack bundles.
var at = require('lodash/at');
var curryN = require('lodash/fp/curryN');
```

실제로는 간단하게 import 구문을 이용하면 좋다.

```js
import _ from 'lodash'
```

### _.find

collection의 모든 요소를 순회하며, predicate 가 true를 반환하는 첫 번째 요소를 반환한다.

```ㅇ
_.find(collection, [predicate=_.identity], [fromIndex=0])
```

위와 같이 사용하면 된다. 구체적으로 설명하자면, collection에는 배열과 같은 자료구조를, predicate는 조건을 반환하는 함수를 작성해준다. fromIndex에는 어느 지점부터 시작할지 index에 해당하는 number를 작성해주면 된다. 

### 활용 사례

```js
let users = [
  { 'user': 'barney',  'age': 36, 'active': true },
  { 'user': 'fred',    'age': 40, 'active': false },
  { 'user': 'pebbles', 'age': 1,  'active': true }
];

_.find(users, function(obj) { return obj.age < 40; });
// object for 'barney'

// The `_.matches` iteratee shorthand.
_.every(users, { 'user': 'barney', 'active': false });
// object for 'pebbles'
```

첫 번째 방법은 조건을 반환하는 함수를 작성해주는 것이다. 위의 경우는 요소의 age 속성의 값이 40보다 아래인 첫 번째 요소를 반환하게 된다.

실제로는 두 번째 방법을 많이 사용했다. 그 이유는 find 메서드 자체가 하나의 요소만을 반환하기 때문이다. 조건으로 찾게 되면 해당 조건에 맞는 요소들 중 첫 번째 요소만을 반환한다. 그래서 구체적인 속성의 값으로 찾는 게 좀 더 직관적인 이해가 가능하다고 생각한다.

# :books:참고자료

https://lodash.com/