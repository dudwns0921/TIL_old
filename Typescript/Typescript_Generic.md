# Typescript

## Polymorphism

**다형성**은 쉽게 말하면 여러 타입을 받아들여 여러 형태의 결과를 가지는 성질을 의미한다. 예시를 통해 살펴보자.

```typescript
type SuperPrint = {
    (arr: number[]):void
}

const superPrint:SuperPrint=(arr)=> {
    arr.forEach((item)=>{console.log(item)})
}

superPrint([1,2,3,4])
```

SuperPrint 타입은 인자의 타입이 number 배열이고, 반환값이 void 타입인 함수이다.

이 타입이 적용된 superPrint 함수는 전달된 배열의 요소들을 전부 출력하는 로직을 가지고 있다. 이 때 함수가 다형성을 가져 number[]뿐만 아니라 boolean[]이나 string[]도 전달받아 출력할 수 있도록 하려면 어떻게 해야할까?

```typescript
type SuperPrint = {
    (arr: number[]):void
    (arr: boolean[]):void
}
```

제일 간단한 방법으로는 SuperPrint 타입을 수정해주는 것이다. 다만 이렇게 하면 출력하고 싶은 모든 타입에 대해 동일한 코드를 작성해주어야 한다는 단점이 있다. 이런 concerete type, 고정된 타입으로 인한 코드 중복 문제는 Generic을 사용해 해결할 수 있다.

## Generic

제네릭(generic)이란 데이터의 타입(data type)을 일반화한다(generalize)는 것을 의미한다. ''일반화한다'' 라는 말이 잘 와닿지 않을 수도 있으니, 구체적인 타입을 지정하는 것이 아니라 다양한 타입을 가질 수 있게끔 한다는 의미 정도로 받아들이면 된다. 사용방법은 다음과 같다.

```typescript
type SuperPrint = {
    <T>(arr: T[]):void
}
superPrint([1,2,3,4])
// const superPrint : <number>(arr:number[])=> void
superPrint([true,false,false,true])
// const superPrint : <boolean>(arr:boolean[])=> void
```

이제 타입스크립트는 주어진 배열을 확인하고 타입을 유추하게 된다. 그러니까 [1,2,3,4]에서는 number[]가 적용되고, [true, false, false, true] 에서는 boolean[]이 적용되는 것이다.

```typescript
type SuperPrint = {
    <T>(arr: T[]):T
}
const superPrint:SuperPrint=(arr)=> {
    return arr[0]
}
const a = superPrint([1,2,3,4])
// const a : number
// const superPrint : <number>(arr:number[])=> number
const b = superPrint([true,false,false,true])
// const b : boolean
// const superPrint : <boolean>(arr:boolean[])=> boolean
const c = superPrint([1,2,true,"hello"])
// const c : number | boolean | string
// const superPrint : <number | boolean | string>(arr:(number | boolean | string)[])=> (number | boolean | string)
```

이런 식으로 반환값도 제네릭을 사용해 처리할 수 있다.

### Any와 다른점?

여러 개의 타입을 받을 수 있다는 점에서 'Any와 큰 차이가 없네?' 라고 생각할 수도 있다. 하지만 둘은 근본적으로 다르다. 예시를 통해 살펴보자.

```typescript
type SuperPrint = {
    (arr: any[]):any

}

const superPrint:SuperPrint=(arr)=> {
    return arr[0]
}

let a = superPrint([1, "b", true]);
a.toUpperCase();
```

```typescript
type SuperPrint = {
    <T>(arr: T[]):T
}

const superPrint:SuperPrint=(arr)=> {
    return arr[0]
}

const a = superPrint([1, "b", true]);
a.toUpperCase();
// Property 'toUpperCase' does not exist on type 'string | number | boolean'.
// Property 'toUpperCase' does not exist on type 'number'
```

any는 타입스크립트로부터 벗어나겠다는 걸 의미한다. 그래서 에러가 발생하지 않으며, a에 string아닌 값이 할당되었을 때만 결과로서 에러를 확인할 수 있다.

Generic은 유추한 타입들을 바탕으로 call signature을 작성한다. 두 번째 예시의 반환값은 string, number, boolean 타입이 될 수 있다. toUpperCase 메서드는 string에만 존재하기 때문에 타입스크립트는 number, boolean이 올 수도 있는 가능성을 염두에 두고 에러를 발생시키는 것이다. 

여러 타입을 받을 수 있다는 점에서 Generic이 굉장히 유연한 것처럼 느껴지지만 결국 우리가 작성한 코드로부터 유추한 타입들만을 받을 수 있다는 점을 알아두자.

# :books:참고자료

노마드코더 강의



