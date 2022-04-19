# 모던자바스크립트

## 챕터 : 브라우저 이벤트 소개

### 과제1

### Hide on click

```html
<body>
  <input type="button" id="hider" value="Click to hide the text" />
  <div id="text">Text</div>
</body>
```

```css
.hidden {
  display: none;
}
```

```javascript
const hideBtn = document.querySelector("#hider");
const txt = document.querySelector("#text");

hideBtn.addEventListener("click", handletheClick);

function handletheClick() {
  const status = txt.classList.toggle("hidden");
  if(status) {
    hideBtn.value = 'Click to show the text';
  } else {
    hideBtn.value = 'Click to hide the text';
  }
}
```

1. hide 버튼을 클릭할 때마다 handletheClick 함수가 실행되게끔 한다.

2. handletheClick함수에서 classList.toggle 함수를 통해 div요소에 hidden 클래스를 추가하거나 제거한다.

3. classList.toggle 함수는 인수로 전달한 클래스가 있고 없고에 따라 true와 false를 반환하는데, 이에 따라 버튼의 텍스트를 바꾼다.

### 과제2

### Move the ball across the field

### 과제3

### Create a sliding menu

```html
<body>
  <span>Sweeties (click me)!</span>
  <ul>
    <li>Cake</li>
    <li>Donut</li>
    <li>Honey</li>
  </ul>
</body>
```

```css
span {
  cursor: pointer;
}

span::before {
  content: '▶';
}

.clicked::before {
  content: '▼';
}

ul {
  display: none;
}

.deactive {
  display: block;
}

```

```javascript
const span = document.querySelector('span');
const ul = document.querySelector('ul');

function handletheClick() {
  span.classList.toggle('clicked');
  ul.classList.toggle('deactive');
}

span.addEventListener('click', handletheClick);
```

classList.toggle 함수를 활용했다는 점에서 과제 1번과 크게 다르지 않다. 다만 ::before 가상 선택자에 대해서 알고 있으면 좀 더 간결하게 문제를 풀 수 있다고 생각한다.
