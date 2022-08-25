# 모던자바스크립트

## 챕터 : 요소 사이즈와 스크롤

### 기하 프로퍼티

![](C:\Users\220307\AppData\Roaming\marktext\images\2022-04-20-08-54-25-image.png)

기하 프로퍼티의 값은 숫자인데 그 단위는 '픽셀’이다. 요소는 아래와 같은 기하 프로퍼티를 지원한다.

- `offsetParent` – 위치 계산에 사용되는 가장 가까운 조상 요소나 `td`, `th`, `table`, `body`
- `offsetLeft`와 `offsetTop` – `offsetParent` 기준으로 요소가 각각 오른쪽, 아래쪽으로 얼마나 떨어져 있는지를 나타내는 값
- `offsetWidth`와 `offsetHeight` – 테두리를 포함 요소 '전체’가 차지하는 너비와 높이
- `clientLeft`와 `clientTop` – 요소 제일 밖을 감싸는 영역과 요소 안(콘텐츠 + 패딩)을 감싸는 영역 사이의 거리를 나타냄. 대부분의 경우 왼쪽, 위쪽 테두리 두께와 일치하지만, 오른쪽에서 왼쪽으로 글을 쓰는 언어가 세팅된 OS에선 `clientLeft`에 스크롤바 두께가 포함됨
- `clientWidth`와 `clientHeight` – 콘텐츠와 패딩을 포함한 영역의 너비와 높이로, 스크롤바는 포함되지 않음
- `scrollWidth`와 `scrollHeight` – `clientWidth`, `clientHeight` 같이 콘텐츠와 패딩을 포함한 영역의 너비와 높이를 나타내는데, 스크롤바에 의해 숨겨진 콘텐츠 영역까지 포함됨
- `scrollLeft`와 `scrollTop` – 스크롤바가 오른쪽, 아래로 움직임에 따라 가려지게 되는 요소 콘텐츠의 너비와 높이

스크롤바를 움직일 수 있게 해주는 `scrollLeft`와 `scrollTop`을 제외한 모든 프로퍼티는 읽기 전용이다.

### 과제1

### Place the ball in the field center

```html
<div id="field">
    <img src="https://en.js.cx/clipart/ball.svg" id="ball"> . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
    . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
</div>
```

```css
#field {
    width: 200px;
    border: 10px groove black;
    background-color: #00FF00;
    position: relative;
  }

  #ball {
    position: absolute;
    width: 40px;
    height: 40px;
  }
```

```javascript
const ball = document.querySelector('#ball');
const field = document.querySelector('#field');

ball.style.top = `${Math.round(
  field.clientHeight / 2 - ball.clientHeight / 2,
)}px`;
ball.style.right = `${Math.round(
  field.clientWidth / 2 - ball.clientWidth / 2,
)}px`;
```

## :bulb:Tip

이 코드는 img 태그에 너비와 높이를 설정하지 않으면 작동하지 않을 수 있다. 브라우저는 이미지가 로드되기 전까지는 이미지의 너비와 높이를 0으로 추정한다.

첫 번째 로드 후, 일반적으로 브라우저는 이미지를 캐시한다. 이미지가 다시 로드될 때 브라우저는 캐시한 이미지의 크기를 바로 가져온다. 하지만 첫 번째 추정된 크기가 0이었기 때문에 이 때 가져오는 크기는 0이 된다.

그래서 css로 너비와 높이를 설정하지 않고 기하 프로퍼티들을 콘솔에 출력해보면 모두 0이 나온다.

여기서 특이하게도 console.dir로 살펴보면 해당 값이 들어있는 걸 확인할 수 있다. 그 이유는 콘솔에 기하 프로퍼티들의 값이 출력되는 시점과 ball 객체가 갱신되는 시점이 차이가 있기 때문이다. 다음과 같은 과정이 진행된다고 보면 된다.

1. 콘솔로 기하 프로퍼티 값을 출력함

2. ball 객체의 프로퍼티 값이 갱신됨

## 챕터 : 브라우저 창 사이즈와 스크롤

### 기하 프로퍼티:

- 사용자 눈에 보이는 문서(콘텐츠가 실제 보여지는 영역)의 너비와 높이: 

​	`document.documentElement.clientWidth/clientHeight`

## :bulb:Tip

### `window` 객체가 아닌 `document.documentElement`를 쓰는 이유

`window.innerWidth/innerHeight`는 스크롤바가 차지하는 영역을 포함해 값을 계산한다. 하지만 창 사이즈가 필요한 경우는 스크롤 바 안쪽에 무언가를 그리거나 위치시킬 때가 대다수이다. 따라서 `documentElement`의 `clientHeight/clientWidth`를 써야 한다.

### 스크롤 관련 프로퍼티:

- 현재 스크롤 정보 읽기: `window.pageYOffset/pageXOffset`.

- 스크롤 상태 변경시키기:
  
  - `window.scrollTo(pageX,pageY)` – 절대 좌표
  - `window.scrollBy(x,y)` – 현재 스크롤 상태를 기준으로 상대적으로 스크롤 정보 변경
  - `elem.scrollIntoView(top)` – `elem`이 눈에 보이도록 스크롤 상태 변경(인수에 따라 창 최상단, 최하단에 해당 요소가 노출되도록 처리)

# :books:참고자료

https://ko.javascript.info/size-and-scroll

https://ko.javascript.info/size-and-scroll-window
