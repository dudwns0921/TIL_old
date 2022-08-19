# DOM 탐색 메서드

## document.getElementById

요소에 `id` 속성이 있으면 위치에 상관없이 메서드 `document.getElementById(id)`를 이용해 접근할 수 있다.

```html
<div id="elem">
  <div id="elem-content">Element</div>
</div>

<script>
  // 요소 얻기
  const elem = document.getElementById('elem');

  // 배경색 변경하기
  elem.style.background = 'red';
</script>
```

또한 id 속성값을 그대로 딴 전역 변수를 이용해 접근할 수도 있다.

```html
<script>
  // 변수 elem은 id가 'elem'인 요소를 참조한다.
  elem.style.background = 'red';

  // id가 elem-content인 요소는 중간에 하이픈(-)이 있기 때문에 변수 이름으로 쓸 수 없다.
  // 이럴 땐 대괄호를 사용해서 window['elem-content']로 접근하면 된다.
  window['elem-content'].style.background = 'blue';
</script>
```

하지만 아래와 같은 단점이 있기 때문에 getElementById 메서드를 사용해 요소에 접근하는 것을 권장한다.

1. id 명과 동일한 이름을 가진 변수가 선언되면 무용지물이 됨

2. 코드만 보고서는 변수의 출처를 바로 알기가 어려움

## querySelectorAll

`elem.querySelectorAll(css)`는 `elem`의 자식 요소 중 주어진 CSS 선택자에 대응하는 요소 모두를 반환한다.

```html
<ul>
  <li>1-1</li>
  <li>1-2</li>
</ul>
<ul>
  <li>2-1</li>
  <li>2-2</li>
</ul>
<script>
const elements = document.querySelectorAll('ul > li:first-child');

for (const elem of elements) {
  console.log(elem.innerHTML);
}
// 1-1
// 2-1
</script>
```

**가상 클래스도 사용할 수 있다.**

querySelectorAll에는 `:hover`나 `:active` 같은 CSS 선택자의 가상 클래스(pseudo-class)도 사용할 수 있다. `document.querySelectorAll(':hover')`을 사용하면 마우스 포인터가 위에 있는(hover 상태인) 요소 모두를 담은 컬렉션이 반환된다. 이때 컬렉션은 DOM 트리 최상단에 위치한 `<html>`부터 가장 하단의 요소 순으로 채워진다.

### querySelector

`elem.querySelector(css)`는 주어진 CSS 선택자에 대응하는 요소 중 첫 번째 요소를 반환한다.

# :books:참고자료

https://ko.javascript.info/searching-elements-dom
