# 디스트럭처링 할당

디스트럭처링 할당(구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructing(비구조화, 구조 파괴) 하여

1개 이상의 변수에 개별적으로 할당하는 것을 의미한다.

이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용

## 배열 디스트럭처링 할당

배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당한다.

이 때 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스이다.

즉, 순서대로 할당된다.

```javascript
const arr = [1,2,3]

const [one, two, three] = arr;

console.log(`${one}, ${two}, ${three}`); // 1,2,3
```

이 때 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없다.

```javascript
const arr = [1,2,3]

const [one, two, three, four] = arr;
const [하나, 둘] = arr;

console.log(`${one}, ${two}, ${three}, ${four}`); // 1,2,3,undefined
console.log(`${하나}, ${둘}`); // 1,2
```

배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.

```javascript
const arr = [1,2,3]

const [one, two, three, four = 4] = arr;

console.log(`${one}, ${two}, ${three}, ${four}`); // 1,2,3,4

// 하지만 기본값보다 할당된 값을 우선한다.

const arr = [1,2,3,"넷"]

const [one, two, three, four = 4] = arr;

console.log(`${one}, ${two}, ${three}, ${four}`); // 1,2,3,넷
```

## 객체 디스트럭처링 할당

객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.

이 때 객체 디스터럭처링 할당의 대상(할당문의 우변)은 객체이어야 하며, 할당 기준은 프로퍼티 키다.

즉, 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.

```javascript
const user = {
    firstName : "Youngjun",
    lastname : "Jung"
}

const {firstName, lastname} = user;

console.log(firstName, lastname); // Youngjun Jung
```

만약 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.

```javascript
const user = {
    firstName : "Youngjun",
    lastname : "Jung"
}

const {firstName:fn, lastname:ln} = user;

console.log(fn, ln); // Youngjun Jung
```

배열과 마찬가지로 객체 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.

```javascript
const {firstName = "Youngjun", lastName} = {lastName : "Jung"};

console.log(firstName, lastName); // Youngjun Jung
```

객체 디스트럭처링 할당은 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.

```javascript
function printTodo({content, completed}) {
    console.log(`할 일 ${content}는 ${completed ? "완료" : "비완료"} 상태입니다.`);
}

printTodo({ id: 1, content: "HTML", completed: true}); //할 일 HTML는 완료 상태입니다.
```

배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.

```javascript
const todos = [
    { id: 1, content: "HTML", completed: true},
    { id: 2, content: "CSS", completed: true},
    { id: 3, content: "Javascript", completed: false}
]

const [{id : id_1}, two, three] = todos;
console.log(id_1, two, three);// 1 {id: 2, content: 'CSS', completed: true} {id: 3, content: 'Javascript', completed: false}
```

중첩 객체의 경우는 다음과 같이 사용한다.

```javascript
const user = {
    name: "Jun",
    address: {
        zipCode: '12345',
        city: 'Seoul'
    }
};

const {address : {zipCode}} = user;
console.log(zipCode); // 12345
```

# :bulb:Tip!

### Renaming을 활용한 값 업데이트

구조 분해 할당 코드를 ()로 감싸서 이미 존재하는 변수의 값을 업데이트할 수 있다.

가령 기존에 사용하던 데이터를 API를 통해 가져온 데이터로 업데이트할 수 있는 것이다.

```javascript
let liveCity = "Seoul";
console.log(liveCity); // Seoul

// 부산으로 이사를 갔다는 정보를 API를 통해 객체로 받음

const data = {
    user: {
        updatedCity: "Busan"
    }
};

({
    user: {
        updatedCity: liveCity
    }
} = data);
console.log(liveCity); // Busan
```

### 변수명 단축

```javascript
const follow = checkFollow();
const alert = checkAlert();

// 기존
const settings = {
notifications: {
follow: follow,
alert: alert,
},
};

// ES6
const settings = {
notifications: {
follow,
alert,
},
};
```

대신 키와 value에 넣을 변수 이름이 같아야 사용가능하다.

### Swapping&Skipping

```javascript
let mon = "Sat"
let sat = "Mon";

[sat, mon] = [mon, sat]

console.log(mon, sat); // Mon Sat
```

배열 디스트럭처링으로 잘못된 변수들의 값을 교환해줄 수 있다.

```javascript
const days = ["mon", "tue", "wed", "thu", "fri"];
const [, , , thu, fri] = days;
console.log(thu, fri); // thu fri
```

콤마를 통해 필요없는 요소들은 생략하고 원하는 요소들만 변수에 할당해줄 수 있다.

# :books:참고자료

이웅모, 모던 자바스크립트 Deep Dive, 위키북스, 2020

노마드코더 수업 내용
