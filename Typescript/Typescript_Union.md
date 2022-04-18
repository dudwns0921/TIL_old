# Union Type

유니온 타입(Union Type)이란 자바스크립트의 OR 연산자(`||`)와 같이 A이거나 B이다 라는 의미의 타입이다.

```typescript
function logText(text: string | number) {
  // ...
}
```

위 함수의 파라미터 `text`에는 문자열 타입이나 숫자 타입이 모두 올 수 있다. 이처럼 `|` 연산자를 이용하여 타입을 여러 개 연결하는 방식을 유니온 타입 정의 방식이라고 부른다.

## Union Type의 장점

유니온 타입의 장점은 아래 2개의 코드를 비교하면 바로 알 수 있다.

```typescript
// any를 사용하는 경우
function getAge(age: any) {
  age.toFixe(); // 에러 발생, age의 타입이 any로 추론되기 때문에 숫자 관련된 API를 작성할 때 코드가 자동 완성되지 않는다.
  return age;
}

// 유니온 타입을 사용하는 경우
function getAge(age: number | string) {
  if (typeof age === 'number') {
    age.toFixed(); // 정상 동작, age의 타입이 `number`로 추론되기 때문에 숫자 관련된 API를 쉽게 자동완성 할 수 있다.
    return age;
  }
  if (typeof age === 'string') {
    return age;
  }
  return new TypeError('age must be number or string');
}
```

이처럼 `any`를 사용하는 경우 마치 자바스크립트로 작성하는 것처럼 동작을 하고 유니온 타입을 사용하면 타입스크립트의 이점을 살리면서 코딩할 수 있다.

## Intersection Type

인터섹션 타입(Intersection Type)은 여러 타입을 모두 만족하는 하나의 타입을 의미한다.

```typescript
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: number;
}
type Jun = Person & Developer;
```

위 코드는 `Person` 인터페이스의 타입 정의와 `Developer` 인터페이스의 타입 정의를 `&` 연산자를 이용하여 합친 후 `Jun` 이라는 타입에 할당한 코드입니다. 결과적으로 `Jun`의 타입은 아래와 같이 정의된다.

```typescript
{
  name: string;
  age: number;
  skill: string;
}
```

이처럼 `&` 연산자를 이용해 여러 개의 타입 정의를 하나로 합치는 방식을 인터섹션 타입 정의 방식이라고 한다.

## Union Type을 쓸 때 주의할 점

앞에서 유니온 타입과 인터섹션 타입을 살펴봤다. 아마 논리적으로 유니온 타입은 OR, 인터섹션은 AND라고 생각하는 사람들이 있을 것이다. 인터페이스와 같은 타입을 다룰 때는 이와 같은 논리적 사고를 주의해야 한다.

```typescript
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: string;
}
function introduce(someone: Person | Developer) {
  someone.name; // O 정상 동작
  someone.age; // X 타입 오류
  someone.skill; // X 타입 오류
}
```

여기서 `introduce()` 함수의 파라미터 타입을 `Person`, `Developer` 인터페이스의 유니온 타입으로 정의했다. 유니온 타입을 A도 될 수 있고 B도 될 수 있는 타입이라고 생각하면 '파라미터의 타입이 `Person`도 되고 `Developer`도 될테니까 함수 안에서 당연히 이 인터페이스들이 제공하는 속성들인 `age`나 `skill`를 사용할 수 있겠지?' 라고 받아들일 수 있다.

하지만, 타입스크립트 관점에서는 `introduce()` 함수를 호출하는 시점에 `Person` 타입이 올지 `Developer` 타입이 올지 알 수가 없기 때문에 어느 타입이 들어오든 간에 오류가 안 나는 방향으로 타입을 추론하게 된다.

```typescript
const jun: Person = { name: 'jun', age: 100 };
introduce(jun); // 만약 `introduce` 함수 안에서 `someone.skill` 속성을 접근하고 있으면 함수에서 오류 발생
```

```typescript
const tony: Developer = { name: 'tony', skill: 'iron making' };
introduce(tony); // 만약 `introduce` 함수 안에서 `someone.age` 속성을 접근하고 있으면 함수에서 오류 발생
```

결과적으로 `introduce()` 함수 안에서는 기본적으로는 `Person`과 `Developer` 두 타입에 공통적으로 들어있는 속성인 `name`만 접근할 수 있게 된다.

```typescript
function introduce(someone: Person | Developer) {
  console.log(someone.name); // O 정상 동작
}
```

# :books: 참고자료

[연산자를 이용한 타입 정의 | 타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/operator.html#union-type)
