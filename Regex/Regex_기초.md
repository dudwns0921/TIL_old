# Regex

## 기초

자바스크립트에서 정규표현식을 만드는 방법은 두 가지가 있다.

**1. 정규 표현식 리터럴**

```js
const re = /ab+c/
```

정규 표현식 리터럴은 스크립트를 불러올 때 컴파일되므로, 바뀔 일이 없는 패턴의 경우 리터럴을 사용하면 성능이 향상될 수 있다.

**2. `RegExp`객체의 생성자 호출**

```js
const re = new RegExp('ab+c')
```

생성자 함수를 사용하면 정규 표현식이 런타임에 컴파일된다.

따라서 바뀔 수 있는 패턴이나, 사용자 입력 등 외부 출처에서 가져오는 패턴의 경우 생성자 함수를 이용하는 것이 좋다.

### 숫자 대표문자

`\d`는 숫자를 대표하는 정규표현식으로, `d`는`digit`을 뜻한다. 

```js
const search_target =
  "Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com";

const re = /\d/g;

console.log([...search_target.matchAll(re)]);

/* 
[
  [
    '0',
    index: 15,
    input: 'Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com',
    groups: undefined
  ],
  [
    '2',
    index: 16,
    input: 'Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com',
    groups: undefined
  ],
  ...
]
*/
```

정규표현식을 통해 위의 문자열에서 숫자만을 반환하도록 했다. 여기서 `matchAll`메서드를 사용했는데, 말 그대로 해당 문자열에서 일치하는 모든 부분을 배열로 반환하는 메서드이다. 주의할 점은 `matchAll`의 경우에는 `g flag`를 작성해주어야 한다는 점이다.

```js
const re = /\d/g;
```

여기서 정규표현식 뒤의 `g`가 `g flag`이다.

### 글자 대표문자

`\w`는 글자를 대표하는 정규표현식이다. 특징은 다음과 같다.

- 문자와 숫자를 포함한다.
- 특수문자는 포함하지 않지만, `_`(언더스코어)는 포함한다.

### 1개 이상

위에서 살펴본 정규표현식들은 모두 한 글자만 반환한다. 만약 전화번호처럼 연결된 숫자를 찾고 싶다면 `+`을 이용하면 된다.

```js
const re = /\d+/g;
```

이 정규표현식은 하나 혹은 그 이상 연결된 숫자를 의미한다. 

```js
const search_target = 위와 동일

const re = /\d+/g;

console.log([...search_target.matchAll(re)]);

/*
['02','123','4567',...]
map메서드로 값만 표시
*/
```

### 0개 이상

#### 자연수 찾기

자연수가 되기 위해서는 아래의 두 가지 조건을 모두 만족해야 한다.

1. 처음 글자가 1~9중 하나의 숫자
2. 그 뒤의 숫자가 0개 이상

여기서 0개 이상은 `*`문자로 표현할 수 있다.

````js
const search_target = 위와 동일

const re = /[1-9]\d*/g;

console.log([...search_target.matchAll(re)].map((item)=>item[0]);

/*
['2','123','4567',...]
*/`
````

### 유무 판단

#### 전화번호 찾기

전화번호는 아래 조건을 만족해야 한다.

1. 연속되는 숫자 사이에 `-`, 혹은 공백이 있거나 없어야 함

여기서 있거나 없어야 한다는 건 `?`로 표현할 수 있다.

```js
const search_target = 위와 동일

const re = /\d+[- ]?\d+[- ]?\d+/g;

console.log([...search_target.matchAll(re)].map((item)=>item[0]);

/*
[ '02-123-4567', '070-9999-9999', '010 2454 3457' ]
*/`
```

하지만 방금 사용한 정규표현식으로는 다음과 같은 문제가 발생한다.

````js
const search_target =
  "이상한 전화번호 0030589-5-95826 Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com";

const re = /\d+[- ]?\d+[- ]?\d+/g;

console.log([...search_target.matchAll(re)].map((item) => item[0]));

// [ '0030589-5-95826', '02-123-4567', '070-9999-9999', '010 2454 3457' ]
````

이렇게 이상한 형태의 전화번호도 반환하게 된다. 이 문제를 해결하기 위해서는 숫자의 개수를 정해줄 필요가 있다.

### n번

`{숫자}`는 `숫자`번 반복한다는 의미이다. 예를 들어 `\d{2}`는 "숫자가 연속 두 번 나온다"는 뜻이다.

이를 활용하면 아까 전화번호를 찾을 때 생긴 문제를 해결할 수 있다.

```js
const search_target =
  "이상한 전화번호 0030589-5-95826 Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com";

const re = /\d{2}[- ]?\d{3}[- ]?\d{4}/g;

console.log([...search_target.matchAll(re)].map((item) => item[0]));

// [ '02-123-4567' ]
```

하지만 여기서도 문제가 발생한다. 물론 `2자리-3자리-4자리` 형태인 전화번호를 찾는 게 목적이었다면 상관이 없겠지만, 이 형태에서 벗어난 다른 전화번호들은 찾을 수가 없다.

### n~m번

`{숫자1, 숫자2}`는 "숫자1부터 숫자2까지 반복한다"는 뜻이다. 예를 들어, `\w{2,3}`는 "문자가 2 ~ 3번 나온다"는 뜻이다. 이를 활용하면 여러 가지 형태의 전화번호들을 모두 찾을 수 있다.

```js
const search_target =
  "이상한 전화번호 0030589-5-95826 Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com";

const re = /\d{2,3}[- ]?\d{3,4}[- ]?\d{4}/g;

console.log([...search_target.matchAll(re)].map((item) => item[0]));
// [ '02-123-4567', '070-9999-9999', '010 2454 3457' ]
```

### 범위에서 고르기

알파벳 중 소문자 모음만 찾고 싶다면 어떻게 해야할까?

`[aeiou]`, 이렇게 표현하는 것을 통해 알파벳 중 소문자 모음만 고를 수 있게 된다.

```js
const search_target =
  "이상한 전화번호 0030589-5-95826 Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com";

const re = /[aeiou]/g;

console.log([...search_target.matchAll(re)].map((item) => item[0]));

//[
  'u', 'e', 'a', 'e', 'u', 'e',
  'a', 'u', 'e', 'a', 'a', 'e',
  'a', 'i', 'o', 'i', 'e', 'e',
  'i', 'a', 'e', 'i', 'a', 'a',
  'i', 'o'
]
```

지금은 알파벳 소문자 모음만 찾았기 때문에 정규표현식 작성이 어렵지 않았지만, 만약 알파벳 소문자 전부를 찾아야 한다면 정규표현식을 작성하는 게 조금 어려워진다.

```js
const re = /[abcdefghijklmnopqrstuvwxyz]/g; // 가독성이 떨어지고 작성하는데 시간이 오래 걸린다.
```

`-`를 사용하는 것으로 위의 정규표현식을 좀 더 간단하게 수정할 수 있다.

```js
const re = /[a-z]/g;
```

같은 원리로 한글을 찾는 정규표현식은 다음과 같이 작성할 수 있다.

```js
const re = /[가-힣]/g;
```

### 기타 대표문자

- \s 공백 문자(스페이스, 탭, 뉴라인)
- \S 공백 문자를 제외한 문자
- \D 숫자를 제외한 문자
- \W 글자 대표 문자를 제외한 글자들(특수문자, 공백 등)

# :books:참고자료

https://school.programmers.co.kr/learn/courses/11

https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_Expressions