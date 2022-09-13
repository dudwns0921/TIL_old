# CSS

## 텍스트 세로 방향 가운데 정렬

### line-height 적용하기

`line-height`는 줄높이를 정하는 속성이다. 주로 텍스트 행 사이의 거리를 설정하는데 사용된다. 이 속성을 통해 어떻게 세로 방향 가운데 정렬을 할 수 있는지 예시를 들어보겠다.

예를 들어 글자크기가 40px일 때 `line-height`의 값이 60px이라고 해보자. 줄 높이는 60px인데 글자 크기는 40px이므로, 글자 위와 아래에 각각 10px의 여백이 생겨 가운데 정렬이 된다.

### padding 적용하기

원리는 `line-height`와 같다. 위아래로 동일한 여백을 적용시켜 세로로 가운데 정렬시키는 방식이다. 주의해야 할 점은 `padding`을 적용하기 위해서는 해당 요소의 `display` 속성의 값이 `block`이어야 한다는 점이다.

### vertical-align 적용하기

`vertical-align` 속성은 `inline` 혹은 `table-cell box`에서의 수직 정렬을 지정한다. 가운데 정렬시키는 `vertical-align` 속성의 값은 `middle`이다. `padding`과 마찬가지로 사용할 때 해당 요소의 `display`값을 꼭 확인해주자.

### 😌마무리

사실 그동안 `flexbox`의 `justify-content`와 `align-items`를 이용해 텍스트 정렬을 구현했다. 물론 이게 틀린 방법은 아니지만, 이 방법은 텍스트 정렬보다는 크기가 가변적인 `container`에서 요소가 항상 동일한 위치를 유지하도록 사용하는 것이 가장 적합하다는 생각이 들었다.
