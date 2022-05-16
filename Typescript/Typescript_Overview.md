# Typescript

## Overview

### Optional Type

optional type은 객체 속성을 필수가 아닌 선택적으로 만들 수 있다. 예시를 통해 살펴보자.

```tsx
const player : {
    name: string,
    age: number
} = {
    name: "Jun",
}

// Errors in code
// Property 'age' is missing in type '{ name: string; }'
// but required in type '{ name: string; age: number; }'.
```

이렇게 정의할 경우, 타입스크립트에서는 위와 같이 에러가 발생한다. number 타입으로 정의한 age 속성이 존재하지 않는다는 의미이다. 서버나 데이터베이스에서 항상 완벽한 데이터를 가져온다는 보장이 있다면 상관이 없겠지만, 분명 위와 같이 몇몇 속성의 값이 제대로 할당되지 않는 경우도 있을 것이다. 이런 문제를 optional type을 통해 해결할 수 있다. 

```tsx
const player : {
    name: string,
    age?: number
} = {
    name: "Jun",
}
```

이렇게 물음표를 사용하면 age는 number 혹은 undefined로 정의될 수 있다는 뜻이다. 그렇기 때문에 더 이상 에러가 발생하지 않는다. 

```tsx
if(player.age<10) {

}

// Errors in code
// Object is possibly 'undefined'

if(player.age && player.age<10) {

}
```

optional type은 위와 같이 안전한 코드를 작성하는데도 도움을 준다. 만약 특정 객체를 찾기 위해 age 속성과 관련된 조건문을 작성했다면, 타입스크립트는 에러를 발생시킨다. 위에서 age 속성은 undefined로 정의될 수도 있다고 했기 때문에, 그 가능성에 대해 언급해주는 것이다.

에러를 발생시키지 않으려면 먼저 age 속성이 있는지부터 판단을 해주어야 하기 때문에 훨씬 더 안전한 코드가 된다.

### Alias(별칭) Type

동일한 Implicit Type(명시적 타입)을 적용된 여러 개의 객체를 만들고 싶다면, 아래와 같이 코드를 작성할 수 있을 것이다.

```tsx
const playerJun : {
    name: string,
    age?: number
} = {
    name: "Jun",
}

const playerRose : {
    name: string,
    age?: number
} = {
    name: "Rose",
}
```

그런데 만약 이런 객체를 몇 천개 만들어야 한다면? 똑같은 코드를 엄청나게 반복해야할 것이다. 그럴 때 우리는 Alias Type을 사용할 수 있다.

```tsx
type Player = {
    name: string,
    age?: number
}

const playerJun : Player = {
    name: "Jun",
}

const playerRose : Player = {
    name: "Rose",
}
```

보다시피 코드가 좀 더 간결해졌다. 타입은 위와 같이 정의하며, 타입의 첫 글자는 대문자로 작성해주어야 한다.

### Return Type

지금까지는 객체에 타입을 적용시키는 방법에 대해 알아봤다. 함수에도 타입을 적용시켜보자. 먼저 Player 객체를 만드는 함수를 작성해보겠다.

```tsx
function PlayerMaker(name:string) {
    return {
        name,
    }
}
const PlayerJun = PlayerMaker("Jun")
PlayerJun.age = 28

// Errors in code
// Property 'age' does not exist on type '{ name : string }'
```

타입스크립트의 관점에서 본다면, PlayerMaker가 반환하는 객체는 name 속성만을 가지는 타입으로 설정된 것이다. 그렇기 때문에 age 속성을 추가하려고 하면 에러가 발생하게 된다. 이를 해결하기 위해서는 반환값에 대한 타입을 제대로 정의해주면 된다.

```tsx
type Player = {
    name: string,
    age?: number
}

function PlayerMaker(name:string) : Player {
    return {
        name,
    }
}
const PlayerJun = PlayerMaker("Jun")
PlayerJun.age = 28
```

위에서 살펴본 Player 타입을 떠올려보자. Player 타입은 name 속성을 가지고, age 속성은 선택적으로 가진다. 따라서 반환값을 Player로 하게 되면 age 속성을 가질 수 있게 되는 것이다.

### Call Signatures

위에서는 함수에 대한 정의와 로직 작성이 함께 이루어졌다. 하지만 Call Signatures 사용한다면 정의와 로직 작성을 분리해서 할 수 있다. 

```tsx
const add = (a:number, b:number) => a+b;

type Add = (a:number, b:number) => number;
const add:Add = (a, b) => a+b
```

### readonly Type

readonly 타입은 자바스크립트에는 존재하지 않는 속성으로 해당 값을 수정할 수 없게끔 만들어주는 보호장치라고 생각하면 된다.

```tsx
type Player = {
    readonly name: string,
    age?: number
}

const PlayerJun = PlayerMaker("Jun")
PlayerJun.name = "newName"

// Errors in code
// Cannot assign to 'name' because it is a read-only property.
```

방금 살펴본 PlayerMaker 함수로 Jun 이라는 이름을 갖는 Player을 만들고 이름을 수정하려고 했다. 그런데 위와 같은 에러가 발생했다. 무엇이 달라진 걸까?

다시 한 번 코드를 확인해보면, Player 타입의 name 속성에 readonly 값이 추가된 것을 확인할 수 있다. name은 읽기 전용이기 때문에 수정을 하려고 하면 타입스크립트에서는 에러를 발생시키게 되는 것이다.

### any Type

보호장치가 있다면 타입스크립트로부터 벗어날 수 있는 장치는 없을까? 타입스크립트는 자바스크립트에 강제성을 부여해 안정성을 높인다. 하지만 우리는 그 강제성만큼 자바스크립트의 자유도를 잃게 되는 것이다. 다시 자바스크립트의 자유도를 되찾고 싶을 때, any 타입을 사용해주면 된다.

```tsx
const a : any[] = [1,2,3,4,5]
const b : any = true

console.log(a+b)

// "1,2,3,4,5true" 
```

### void

void는 값을 반환하지 않는 함수의 반환 타입을 나타낸다. 함수에 return 문이 없거나 해당 return 문에서 명시적 값을 반환하지 않을 때 항상 유추되는 타입이다. 
```tsx
function echo(message:string) {
    console.log(message)
}

const a = echo("Hello")
a.toUpperCase()

// Errors in code
// Property 'to' does not exist on type 'void'.
```

void 타입에 string 타입에 있는 toUpperCase 메서드를 실행시키려고 하면 에러가 발생한다. void 타입이라고 해서 타입이 정의되지 않은 게 아니라는 점을 알아두자. null과 같은 의미라고 생각하면 좋을 것 같다.

### unknown

unknown타입은 모든 값을 나타낸다. 이것은 any타입과 비슷하지만 unknown이 더 안전한데, 그 이유는 예시를 통해 알아보자.

```tsx
let a : unknown;

if(typeof(a)==="number") {
    let b = a+1
}

if(typeof(a)==="string") {
    console.log(a.toUpperCase())
}
```

unknown으로 타입을 설정하면 타입스크립트가 타입확인을 강제하기 때문에 좀 더 안전하게 작업을 할 수 있게 된다.

### never

일부 함수는 값을 반환하지 않는다. 이는 함수가 예외를 throw하거나 프로그램 실행을 종료함을 의미한다.
```tsx
function fail(message: string): never {
	throw new Error(message);
}
```

### Call Signatures

쉽게 말하면 함수 전체에 대한 타입을 정의하는 것이다. 함수 타입에 대한 정의와 구현을 분리해서 할 수 있다는 장점이 있다. 

```tsx
const add = (a:number, b:number) => a+b;

type Add = (a:number, b:number) => number;
const add:Add = (a, b) => a+b
```

# :books:참고자료

노마드코더 강의

https://www.typescriptlang.org/docs/handbook/2/functions.html