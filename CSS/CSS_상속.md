# Learn CSS!

## 챕터 : 상속

### 상속 원리

HTML 요소의 CSS 속성에는 기본값이 설정되어있다. 기본값은 상속되지 않으며 상속에 의해 HTML 요소의 속성값이 산출되지 않을 경우에 표시된다.

상속의 흐름은 무조건 아래쪽을 향한다. 그래서 상속 가능한 속성의 경우 자식 요소가 부모 요소 속성의 값을 갖게 된다.

하지만 자식 요소에서 새롭게 해당 속성의 값을 정의하거나 user agent stylesheet(각 브라우저마다 정해놓은 CSS 기본 규칙)에 의해 값이 정의되어 있다면 상속은 일어나지 않는다.

### 상속 가능한 속성들

모든 CSS 속성들이 상속 가능하지는 않지만, 많은 속성들이 상속 가능하다. 다음은 W3 관련 문서에서 가져온 상속 가능한 모든 CSS 속성들이다:

- [azimuth](https://developer.mozilla.org/docs/Web/SVG/Attribute/azimuth)
- [border-collapse](https://developer.mozilla.org/docs/Web/CSS/border-collapse)
- [border-spacing](https://developer.mozilla.org/docs/Web/CSS/border-spacing)
- [caption-side](https://developer.mozilla.org/docs/Web/CSS/caption-side)
- [color](https://developer.mozilla.org/docs/Web/CSS/color)
- [cursor](https://developer.mozilla.org/docs/Web/CSS/cursor)
- [direction](https://developer.mozilla.org/docs/Web/CSS/direction)
- [empty-cells](https://developer.mozilla.org/docs/Web/CSS/empty-cells)
- [font-family](https://developer.mozilla.org/docs/Web/CSS/font-family)
- [font-size](https://developer.mozilla.org/docs/Web/CSS/font-size)
- [font-style](https://developer.mozilla.org/docs/Web/CSS/font-style)
- [font-variant](https://developer.mozilla.org/docs/Web/CSS/font-variant)
- [font-weight](https://developer.mozilla.org/docs/Web/CSS/font-weight)
- [font](https://developer.mozilla.org/docs/Web/CSS/font)
- [letter-spacing](https://developer.mozilla.org/docs/Web/CSS/letter-spacing)
- [line-height](https://developer.mozilla.org/docs/Web/CSS/line-height)
- [list-style-image](https://developer.mozilla.org/docs/Web/CSS/list-style-image)
- [list-style-position](https://developer.mozilla.org/docs/Web/CSS/list-style-position)
- [list-style-type](https://developer.mozilla.org/docs/Web/CSS/list-style-type)
- [list-style](https://developer.mozilla.org/docs/Web/CSS/list-style)
- [orphans](https://developer.mozilla.org/docs/Web/CSS/orphans)
- [quotes](https://developer.mozilla.org/docs/Web/CSS/quotes)
- [text-align](https://developer.mozilla.org/docs/Web/CSS/text-align)
- [text-indent](https://developer.mozilla.org/docs/Web/CSS/text-indent)
- [text-transform](https://developer.mozilla.org/docs/Web/CSS/text-transform)
- [visibility](https://developer.mozilla.org/docs/Web/CSS/visibility)
- [white-space](https://developer.mozilla.org/docs/Web/CSS/white-space)
- [widows](https://developer.mozilla.org/docs/Web/CSS/widows)
- [word-spacing](https://developer.mozilla.org/docs/Web/CSS/word-spacing)

### 상속 제어

상속은 예상치 못한 방식으로 요소에 영향을 줄 수 있으므로 CSS에서는 3가지 키워드로 상속을 제어한다.

#### inherit

inherit 키워드를 사용한 속성은 부모 요소로부터 해당 속성의 계산된 값을 받는다. 상속되는 속성과 상속되지 않는 속성 모두에 작동한다.

#### initial

initial 키워드를 적용한 속성은 속성의 기본값을 요소에 적용한다.

#### unset

unset 키워드는 부모로부터 상속할 값이 존재하면, 상속값을, 그렇지 않다면 기본값을 사용한다. 



# :books:참고자료

[Inheritance](https://web.dev/learn/css/inheritance/)


