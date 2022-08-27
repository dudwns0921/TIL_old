# 모던자바스크립트

## 챕터 : 버블링과 캡처링

표준 [DOM 이벤트](http://www.w3.org/TR/DOM-Level-3-Events/)에서 정의한 이벤트 흐름엔 3가지 단계가 있다.

1. 캡처링 단계 – 이벤트가 하위 요소로 전파되는 단계
2. 타깃 단계 – 이벤트가 실제 타깃 요소에 전달되는 단계
3. 버블링 단계 – 이벤트가 상위 요소로 전파되는 단계

공식적으론 이렇게 총 3개의 이벤트 흐름이 있지만, 이벤트가 실제 타깃 요소에 전달되는 단계인 ‘타깃 단계’(두 번째 단계)는 별도로 처리되지 않는다. 

### 버블링

버블링(bubbling)의 원리는 매우 간단하다. 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작한다. 가장 최상단의 조상 요소를 만날 때까지 이 과정이 반복된다.

대부분의 이벤트는 버블링되지만 focus와 같이 버블링되지 않는 이벤트들도 존재한다.

### event.target과 event.currentTarget

부모 요소의 핸들러는 이벤트가 정확히 어디서 발생했는지 등에 대한 자세한 정보를 얻을 수 있다. 이벤트가 발생한 가장 안쪽의 요소는 타깃(target) 요소라고 불리고, `event.target`을 사용해 접근할 수 있다.

`event.target`과 `this`(=`event.currentTarget`)는 다음과 같은 차이점이 있다.

- `event.target`은 실제 이벤트가 시작된 ‘타깃’ 요소이다. 버블링이 진행되어도 변하지 않습니다.
- `this`는 ‘현재’ 요소로, 현재 실행 중인 핸들러가 할당된 요소를 참조한다.

예시로 살펴보자.

```html
<form id="form">FORM
    <div>DIV
        <p>P</p>
    </div>
</form>
```

```css
form {
    background-color: green;
    position: relative;
    width: 150px;
    height: 150px;
    text-align: center;
    cursor: pointer;
  }

  div {
    background-color: blue;
    position: absolute;
    top: 25px;
    left: 25px;
    width: 100px;
    height: 100px;
  }

  p {
    background-color: red;
    position: absolute;
    top: 25px;
    left: 25px;
    width: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0;
  }

  body {
    line-height: 25px;
    font-size: 16px;
  }
```

```javascript
const form = document.querySelector('#form');

form.onclick = function (event) {
  event.target.style.backgroundColor = 'yellow';

  // chrome needs some time to paint yellow
  setTimeout(() => {
    alert('target = ' + event.target.tagName + ', this=' + this.tagName);
    event.target.style.backgroundColor = '';
  }, 0);
};
```

P를 클릭하게 되면 alert로 다음과 같은 메시지가 나온다.

**target = P, this=FORM**

먼저 버블링의 개념을 다시 상기시켜보자. 코드를 보면 실제로 이벤트 핸들러가 있는 곳은 form 태그 뿐이다. p 태그를 클릭했음에도 핸들러가 작동한 이유는 이벤트가 발생한 p 태그부터 document 객체를 만날 때까지 계속 이벤트가 위로 버블링되기 때문이다.

그래서 target과 this는 결과값이 다를 수밖에 없다. 실제로 이벤트가 발생한 곳인 target은 p 태그이며, 현재 실행 중인 핸들러가 할당된 곳인 this는 form 태그에 해당한다. 만약 form 태그를 클릭하게 되면 target과 this의 값은 같다.

### 캡처링

버블링과 반대로 이벤트가 최상위 조상에서 시작해 아래로 전파되는 과정이다.

`on<event>` 프로퍼티나 HTML 속성, `addEventListener(event, handler)`를 이용해 할당된 핸들러는 캡처링에 대해 전혀 알 수 없다. 이 핸들러들은 두 번째 혹은 세 번째 단계의 이벤트 흐름(타깃 단계와 버블링 단계)에서만 동작한다.

캡처링 단계에서 이벤트를 잡아내려면 `addEventListener`의 `capture` 옵션을 `true`로 설정해야 한다.

```javascript
elem.addEventListener(..., {capture: true}) ;
// 아니면, 아래 같이 {capture: true} 대신, true를 써줘도 된다.
elem.addEventListener(...,true);
```

# :books:참고자료

https://ko.javascript.info/bubbling-and-capturing
