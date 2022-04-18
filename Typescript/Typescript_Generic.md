# Generic

Java나 C# 같은 정적 타입 언어의 경우, 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언해야 한다. TypeScript 또한 정적 타입 언어이기 때문에 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언해야 한다. 그런데 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언하기 어려운 경우가 있다. 이럴 때 사용하는 것이 바로 제네릭이다.

**제네릭은 선언 시점이 아니라 생성 시점에 타입을 명시하여 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 기법이다. 한번의 선언으로 다양한 타입에 재사용이 가능하다는 장점이 있다.**

**T는 제네릭을 선언할 때 관용적으로 사용되는 식별자로 타입 파라미터(Type parameter)라 한다.** T는 Type의 약자로 반드시 T를 사용해야 하는 것은 아니다.

또한 함수에도 제네릭을 사용할 수 있다. 제네릭을 사용하면 하나의 타입만이 아닌 다양한 타입의 매개변수와 리턴값을 사용할 수 있다.

## 사용 방법

```typescript
function log<T>(value: T): T {
  return value;
}
```

위 함수는 제네릭 기본 문법이 적용된 형태이다. 이제 함수를 호출할 때 아래와 같이 함수 안에서 사용할 타입을 넘겨줄 수 있다.

```typescript
log<string>('hi');
log<number>(10);
log<boolean>(true);
```

위 코드는 모두 정상 작동하며, 그 중 `getText<string>('hi')`를 호출 했을 때 함수에서 제네릭이 어떻게 동작하는지 살펴보자.

```typescript
function log<string>(value: string): string {
  return value;
}
```

결과적으로는 위와 같은 타입을 정의한 것과 같다. 제네릭으로 사용할 타입을 string으로 넘겨줬기 때문에 입력 값의 타입과 반환 값의 타입도 string이어야 한다.

## 

앞에서 배운 내용으로 제네릭을 사용하기 시작하면 컴파일러에서 인자에 타입을 넣어달라는 경고를 보게 됩니다.

조금 전에 살펴본 코드를 다시 보겠습니다.

```
function logText<T>(text: T): T {
  return text;
}
```

만약 여기서 함수의 인자로 받은 값의 `length`를 확인하고 싶다면 어떻게 해야 할까요? 아마 아래와 같이 코드를 작성할 겁니다.

```
function logText<T>(text: T): T {
  console.log(text.length); // Error: T doesn't have .length
  return text;
}
```

위 코드를 변환하려고 하면 컴파일러에서 에러를 발생시킵니다. 왜냐하면 `text`에 `.length`가 있다는 단서는 어디에도 없기 때문이죠.

다시 위 제네릭 코드의 의미를 살펴보면 함수의 인자와 반환 값에 대한 타입을 정하진 않았지만, 입력 값으로 어떤 타입이 들어왔고 반환 값으로 어떤 타입이 나가는지 알 수 있습니다. 따라서, 함수의 인자와 반환 값 타입에 마치 `any`를 지정한 것과 같은 동작을 한다는 것을 알 수 있죠. 그래서 설령 인자에 `number` 타입을 넘기더라도 에러가 나진 않습니다. 이러한 특성 때문에 현재 인자인 `text`에 문자열이나 배열이 들어와도 아직은 컴파일러 입장에서 `.length`를 허용할 순 없습니다. 왜냐하면 `number`가 들어왔을 때는 `.length` 코드가 유효하지 않으니까요.

그래서 이런 경우에는 아래와 같이 제네릭에 타입을 줄 수가 있습니다.

```
function logText<T>(text: T[]): T[] {
  console.log(text.length); // 제네릭 타입이 배열이기 때문에 `length`를 허용합니다.
  return text;
}
```

위 코드가 기존의 제네릭 코드와 다른 점은 인자의 `T[]` 부분입니다. 이 제네릭 함수 코드는 일단 `T`라는 변수 타입을 받고, 인자 값으로는 배열 형태의 `T`를 받습니다. 예를 들면, 함수에 `[1,2,3]`처럼 숫자로 이뤄진 배열을 받으면 반환 값으로 `number`를 돌려주는 것이죠. 이런 방식으로 제네릭을 사용하면 꽤 유연한 방식으로 함수의 타입을 정의해줄 수 있습니다.

혹은 다음과 같이 좀 더 명시적으로 제네릭 타입을 선언할 수 있습니다.

```
function logText<T>(text: Array<T>): Array<T> {
  console.log(text.length);
  return text;
}
```

## [#](https://joshua1988.github.io/ts/guide/generics.html#%EC%A0%9C%EB%84%A4%EB%A6%AD-%ED%83%80%EC%9E%85)제네릭 타입

제네릭 인터페이스에 대해 알아보겠습니다. 아래의 두 코드는 같은 의미입니다.

```
function logText<T>(text: T): T {
  return text;
}
// #1
let str: <T>(text: T) => T = logText;
// #2
let str: {<T>(text: T): T} = logText;
```

위와 같은 변형 방식으로 제네릭 인터페이스 코드를 다음과 같이 작성할 수 있습니다.

```
interface GenericLogTextFn {
  <T>(text: T): T;
}
function logText<T>(text: T): T {
  return text;
}
let myString: GenericLogTextFn = logText; // Okay
```

위 코드에서 만약 인터페이스에 인자 타입을 강조하고 싶다면 아래와 같이 변경할 수 있습니다.

```
interface GenericLogTextFn<T> {
  (text: T): T;
}
function logText<T>(text: T): T {
  return text;
}
let myString: GenericLogTextFn<string> = logText;
```

이와 같은 방식으로 제네릭 인터페이스 뿐만 아니라 클래스도 생성할 수 있습니다. 다만, *이넘(enum)과 네임스페이스(namespace)는 제네릭으로 생성할 수 없습니다*.



제네릭 클래스는 앞에서 살펴본 제네릭 인터페이스와 비슷합니다. 코드를 보겠습니다.

```
class GenericMath<T> {
  pi: T;
  sum: (x: T, y: T) => T;
}

let math = new GenericMath<number>();
```

제네릭 클래스를 선언할 때 클래스 이름 오른쪽에 `<T>`를 붙여줍니다. 그리고 해당 클래스로 인스턴스를 생성할 때 타입에 어떤 값이 들어갈 지 지정하면 됩니다.

조금 전에 살펴본 인터페이스처럼 제네릭 클래스도 클래스 안에 정의된 속성들이 정해진 타입으로 잘 동작하게 보장할 수 있습니다.

WARNING

참고! Generic classes are only generic over their instance side rather than their static side, so when working with classes, static members can not use the class’s type parameter

## [#](https://joshua1988.github.io/ts/guide/generics.html#%EC%A0%9C%EB%84%A4%EB%A6%AD-%EC%A0%9C%EC%95%BD-%EC%A1%B0%EA%B1%B4)제네릭 제약 조건

앞에서 [제네릭 타입 변수](https://joshua1988.github.io/ts/guide/generics.html#%EC%A0%9C%EB%84%A4%EB%A6%AD-%ED%83%80%EC%9E%85-%EB%B3%80%EC%88%98)에서 살펴본 내용 말고도 제네릭 함수에 어느 정도 타입 힌트를 줄 수 있는 방법이 있습니다. 잠시 이전 코드를 보겠습니다.

```
function logText<T>(text: T): T {
  console.log(text.length); // Error: T doesn't have .length
  return text;
}
```

인자의 타입에 선언한 `T`는 아직 어떤 타입인지 구체적으로 정의하지 않았기 때문에 `length` 코드에서 오류가 납니다. 이럴 때 만약 해당 타입을 정의하지 않고도 `length` 속성 정도는 허용하려면 아래와 같이 작성합니다.

```
interface LengthWise {
  length: number;
}

function logText<T extends LengthWise>(text: T): T {
  console.log(text.length);
  return text;
}
```

위와 같이 작성하게 되면 타입에 대한 강제는 아니지만 `length`에 대해 동작하는 인자만 넘

### [
