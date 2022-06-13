# Typescript

## Type Assertions

때로는 타입스크립트보다 사용자가 어떤 값의 타입에 대한 정보를 더 잘 아는 경우도 있다.

예를 들어 코드상에서 `document.getElementById`가 사용되는 경우, 타입스크립트는  `HTMLElement` 중에 무언가가 반환된다는 것만을 알 수 있다. 하지만 사용자는 특정 ID로는 언제나 `HTMLCanvasElement`가 반환된다는 사실을 알 수도 있다.

이런 경우, 타입 단언을 사용하면 타입을 좀 더 구체적으로 명시할 수 있다.

```js
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

타입 단언은 컴파일러에 의하여 제거되며 코드의 런타임 동작에는 영향을 주지 않는다. 코드가 `.tsx` 파일에 작성되지 않은 경우, 꺾쇠괄호를 사용할 수도 있다.

```js
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

## :bulb:Tip

타입 단언은 컴파일 시간에 제거되므로, 타입 단언에 관련된 검사는 런타임 중에 이루어지지 않는다. 타입 단언이 틀렸더라도 예외가 발생하거나 `null`이 생성되지 않을 것이다.

____

타입스크립트에서는 보다 구체적인 또는 덜 구체적인 버전의 타입으로 변환하는 타입 단언만이 허용된다. 이러한 규칙은 아래와 같은 “불가능한” 강제 변환을 방지한다.

```js
const x = "hello" as number;
```

# :books:참고자료

https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%8B%A8%EC%96%B8

