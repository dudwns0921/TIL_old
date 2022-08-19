# Javascript

회사에서 원인을 알 수 없는 버그가 발생했다.

```js
// Music app
...
public async play(track) {
  if (this._isLoadedTrack(track)) {
    return this._audioElement.play()
  }
...
```

문제는 Music app의 외부로부터 음악 관련 데이터들을 받아오고 재생, 멈춤 등의 동작을 수행하는 MediaProxy라는 클래스 안에 정의된 메서드에서 발생했다. 

로직을 간단하게 정리하자면 이미 재생중인 트랙과 재생하려는 트랙을 비교해 같을 경우에는 그냥 바로 재생시키는 로직이다. 그런데 재생 후 갑작스럽게 `pause`이벤트가 발생해 재생이 되지 않았다. 왜 이런 문제가 발생하는지 도저히 알 수가 없어 이런저런 방법들을 시도해보다가 다음과 같이 수정했을 때 정상적으로 재생되는 걸 확인했다.

```js
// Music app
...
public async play(track) {
  if (this._isLoadedTrack(track)) {
  	this._audioElement.play()
    return
  }
...
```

기존 코드에서 함수를 반환했다면, 수정한 코드에서는 함수를 실행시킨 후에 `return`을 통해 함수를 중단시키고 있다. 함수를 반환하는 코드 자체를 많이 보지 못해 좀 더 알아봐야겠다는 생각이 들었다.

## Closure

구글링을 하다가 아래의 자료를 발견했다.

> [return function 함수를 리턴하는 기법은 어디에 사용될까](https://inpa.tistory.com/entry/JS-%F0%9F%92%A1-return-function-%ED%95%A8%EC%88%98%EB%A5%BC-%EB%A6%AC%ED%84%B4%ED%95%98%EB%8A%94-%EA%B8%B0%EB%B2%95%EC%9D%80-%EC%96%B4%EB%94%94%EC%97%90-%EC%82%AC%EC%9A%A9%EB%90%A0%EA%B9%8C)

여기서 함수를 리턴하는 방식이 클로저에서 사용된다는 걸 확인했고, 클로저에 대해 알아보게 되었다.

___

> 클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다. 

클로저를 이해하기 위해서는 자바스크립트가 어떻게 변수의 유효범위를 지정하는지(Lexical Scoping)에 대해서 이해해야 한다.

### Lexical Scoping

```js
function init() {
  var name = "Jun"; // name은 init에 의해 생성된 지역 변수이다.
  function displayName() {
    // displayName() 은 내부 함수이다.
    console.log(name); // 부모 함수에서 선언된 변수를 사용한다.
  }
  displayName();
}
init();

// Jun
```

1. `init()` 이 실행되며 지역 변수인 `name`과 `displayName()` 함수가 생성된다.
2. `displayName()`함수가 실행되며 문자열 'Jun'이 출력된다.

여기서 중요한 점은 내부 함수에서 외부 함수인 `init()`에서 정의한 지역변수인 `name`을 사용했다는 점이다. 만약 `displayName()`함수가 자신만의 `name`변수를 가지고 있었다면 `this.name`을 사용했을 것이다.

이 예시를 통해 함수가 중첩된 상황에서 파서가 어떻게 변수를 처리하는지 알 수 있다. 이는 `lexical scoping`의 한 예시이며, `lexical scoping`은 변수가 어디에서 사용 가능한지 알기 위해 그 변수가 소스 코드 내 어디에서 선언되었는지 고려한다. 

```js
var name = "Jun"; // name은 외부 범위에서 선언한 변수이다.
function init() {
  function displayName() {
    // displayName() 은 내부 함수이다.
    console.log(name); // 외부 범위에서 선언된 변수를 사용한다.
  }
  displayName();
}
init();
```

추가적으로 중첩된 함수는 외부 범위에서 선언한 변수에도 접근할 수 있다.

### Closure

```js
function makeFunc() {
  var name = "Jun";
  function displayName() {
    console.log(name);
  }
  return displayName;
}

var myFunc = makeFunc();
//myFunc변수에 displayName을 리턴함
//유효범위의 어휘적 환경을 유지
myFunc();
//리턴된 displayName 함수를 실행(name 변수에 접근)
```

1. `myFunc`변수에 `displayName()` 함수를 리턴한다.
2. `myFunc` 변수를 실행시키면 `displayName()` 함수가 실행되어 문자열 'Jun' 이 콘솔에 출력한다.

결과는 바로 전의 예시와 동일하지만, 둘의 차이는 `displayName()`함수가 실행되기 전에 외부함수인 `makeFunc()`로부터 리턴되어 `myFunc` 변수에 저장된다는 것이다.

몇몇 프로그래밍 언어에서 함수 안의 지역 변수들은 그 함수가 처리되는 동안에만 존재한다. `makeFunc()` 실행이 끝나면 `name` 변수에 더 이상 접근할 수 없게 될 것으로 예상하는 것이 일반적이다.

하지만 자바스크립트의 경우는 다르다. 그 이유는 자바스크립트는 함수를 반환할 때, 반환하는 함수가 클로저를 형성하기 때문이다.

**클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다.** 이 환경은 클로저가 생성된 시점의 유효 범위 내에 있는 모든 지역 변수로 구성된다.

예시를 다시 한 번 살펴보자. `myFunc` 변수는 `makeFunc()` 함수가 실행 될 때 생성된 `displayName()` 함수의 인스턴스에 대한 참조다. `displayName()`의 인스턴스는 변수 `name` 이 있는 어휘적 환경에 대한 참조를 유지한다. 이런 이유로 `myFunc`가 호출될 때 변수 `name`은 사용할 수 있는 상태로 남게 되고 'Jun' 이 `console.log` 에 전달된다.

### 실용적인 Closure

클로저는 어떤 데이터(어휘적 환경)와 그 데이터를 조작하는 함수를 연관시켜주기 때문에 유용하다고 하는데, 많이 사용해보지 않아 와닿지는 않았다. 실제 예시를 통해 어떻게 클로저를 실용적으로 사용하는지 알아보자.

```html
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
   <button id="c1">C1</button>
   <button id="c2">C2</button>
   <button id="c3">C3</button>
 
   <script>
      document.getElementById('c1').addEventListener('click', function(){ console.log(1) });
      document.getElementById('c2').addEventListener('click', function(){ console.log(1) });
      document.getElementById('c3').addEventListener('click', function(){ console.log(1) });
   </script>
</body>
</html>
```

3개의 버튼이 있고, 이벤트 리스너를 통해 클릭시 1이 콘솔에 출력된다. 코드가 중복되기 때문에 함수를 밖으로 꺼내보겠다.

```html
   <script>
      function consoleOne() {
        console.log(1)
      }
      document.getElementById('c1').addEventListener('click', consoleOne);
      document.getElementById('c2').addEventListener('click', consoleOne);
      document.getElementById('c3').addEventListener('click', consoleOne);
   </script>
```

위의 예시처럼 이벤트 리스너에는 함수 자체가 들어가야 한다. `consoleOne()`의 형태로 넣으면 반환값이 들어가기 때문에 함수 실행이 되지 않기 때문이다.

이제 아래의 요구사항을 추가해보자.

> 각 버튼마다 다른 값을 출력해야 한다.

가장 간단하게 생각할 수 있는 방법은 파라미터로 각기 다른 값을 전달하는 것이다.

```js
function consoleOne(num) {
  console.log(num)
}
document.getElementById('c1').addEventListener('click', consoleOne(1));
```

앞서 말했지만, 이벤트 리스너에 `function()` 의 형태로 함수를 전달하게 되면 반환값을 넣어버리는 꼴이 된다. 따라서 버튼을 누르지도 않았는데 console.log(1) 이 실행돼 버린다. 이 문제는 클로저를 통해 해결할 수 있다.

```js
function returnConsoleNum(Num) {
  return function consoleNum() {
    console.log(Num)
  }
}
 
document.getElementById('c1').addEventListener('click', returnConsoleNum(1));
document.getElementById('c2').addEventListener('click', returnConsoleNum(2));
document.getElementById('c3').addEventListener('click', returnConsoleNum(3));
```

이렇게 하면 이벤트 리스너에는 반환값인 함수가 전달된다. 그리고 이벤트 리스너에 등록된 함수들은 모두 클로저로 같은 함수 본문 정의를 공유하지만 서로 다른 맥락적(어휘적) 환경을 저장한다.

`returnConsoleNum(1)`의 맥락적 환경에서는 Num은 1이기 때문에 콘솔에 1이 출력되고, 같은 원리로 `returnConsoleNum(2)`는 콘솔에 2를 출력한다.

## 마무리

이것저것 찾다보니 발생한 문제와는 좀 동떨어진 내용까지 학습하게 된 것 같다. 다만 `Closure`는 프론트엔드 개발자라면 꼭 알고 있어야 하는 지식인만큼 이번 기회를 통해 알아본 것이 다행이라고 생각한다. 

# :books:참고자료

https://inpa.tistory.com/entry/JS-%F0%9F%92%A1-return-function-%ED%95%A8%EC%88%98%EB%A5%BC-%EB%A6%AC%ED%84%B4%ED%95%98%EB%8A%94-%EA%B8%B0%EB%B2%95%EC%9D%80-%EC%96%B4%EB%94%94%EC%97%90-%EC%82%AC%EC%9A%A9%EB%90%A0%EA%B9%8C

https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures